---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-18"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}

# Using {{site.data.keyword.sqlquery_notm}} as external metastore 
{: #data-engine-external-metastore}

[Deprecated]{: tag-deprecated}

{{site.data.keyword.sqlquery_notm}} is IBM Cloud's central service for data lakes. It provides stream ingestion, data preparation, ETL, and data query from {{site.data.keyword.cos_full_notm}} and Kafka. It also manages tables and views in a catalog that is compatible with Hive metastore and other big data engines and services can connect to it. See [Overview of {{site.data.keyword.sqlquery_notm}}](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview).
{: shortdesc}

Each instance of {{site.data.keyword.sqlquery_notm}} includes a database catalog that you can use to register and manage table definitions for your data on {{site.data.keyword.cos_full_notm}}. Catalog syntax is compatible with Hive metastore syntax. You can use {{site.data.keyword.sqlquery_notm}} to externalize metadata outside the {{site.data.keyword.iae_full_notm}} Spark cluster.

## Pre-requisites
{: #metastore-prerequisite}

The following are the pre-requisites:

* Creating IBM Cloud Data Engine instance
* Storing data in Cloud Object Storage
* Creating schema

### **Creating IBM Cloud Data Engine instance**
{: #metastore-prerequisite_line1}


Create an {{site.data.keyword.sqlquery_notm}} instance by using the Standard plan. See [{{site.data.keyword.sqlquery_short}}](http://cloud.ibm.com/catalog/services/data-engine-previously-sql-query).

After you have provisioned the {{site.data.keyword.sqlquery_short}} instance:
1. Make a note of the CRN of the instance.
1. Create an account-level API key or service ID level API key with access to the instance.
1. This service ID should be granted access to both the {{site.data.keyword.sqlquery_short}} instance as well as the {{site.data.keyword.cos_full_notm}} bucket.

You can then configure your {{site.data.keyword.iae_full_notm}} instance to use the default metastore configuration either at instance level or at application level as needed.

{{site.data.keyword.sqlquery_notm}} supports creating instances for different endpoints(location). Within an instance, different IBM Cloud Object Storage buckets are created to store data. The data buckets can be created for different end points(region). The endpoints for the data engine instance(thrift) and the data bucket are different. Ensure that you select the correct endpoints that are supported by the system.\
• For more information about the applicable endpoint(thrift) for your region while creating instance, see [Thrift endpoint](https://cloud.ibm.com/docs/sql-query?topic=sql-query-hive_metastore#hive_compatible_client).\
• For more information on the currently supported data engine endpoints, see [Data engine endpoints](#aeendpoints).
{: important}

### **Storing data in Cloud Object Storage**
{: #metastore-prerequisite_line2}

Generate and store data in cloud object storage. Run the following regular PySpark application, called `generate-and-store-data.py` in this example, which stores Parquet data in some location on {{site.data.keyword.cos_full_notm}}.

Example:

Enter:

```python
from pyspark.sql import SparkSession

def init_spark():
    spark = SparkSession.builder.appName("dataengine-generate-store-parquet-data").getOrCreate()
    sc = spark.sparkContext
    return spark,sc

def generate_and_store_data(spark,sc):
    data =[("India","New Delhi"),("France","Paris"),("Lithuania","Vilnius"),("Sweden","Stockholm"),("Switzerland","Bern")]
    columns=["Country","Capital"]
    df=spark.createDataFrame(data,columns)
    df.write.mode("overwrite").parquet("cos://mybucket.mycosservice/countriescapitals.parquet")

def main():
    spark,sc = init_spark()
    generate_and_store_data(spark,sc)
if __name__ == '__main__':
    main()
```
{: codeblock}

### **Creating schema**
{: #metastore-prerequisite_line3}

Create the metastore table schema definition in the data engine. Note that you can't use standard Spark SQL syntax to create tables when using {{site.data.keyword.sqlquery_short}} as a metastore. There are two ways that you can use to create a table:

- From the {{site.data.keyword.sqlquery_short}} user interface or by using, the standard {{site.data.keyword.sqlquery_short}} API (see [Data Engine service REST V3 API](https://cloud.ibm.com/apidocs/sql-query-v3#introduction)) or Python SDK (see [ibmcloudsql](https://pypi.org/project/ibmcloudsql/)).

    Example:

    Enter:

    ```bash
        CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) 
        USING PARQUET 
        LOCATION cos://us-south/mybucket/countriescapitals.parquet
    ```
    {: codeblock}


    In the above example, the location (//us-south/mybucket/countriescapitals.parquet) of the COS bucket is considered as `us-south`, which is the regional bucket. If you are using any other region, select the corresponding alias from the [Data engine endpoints](#aeendpoints).


- Programmatically from within your PySpark application by using the following code snippet for PySpark called `create_table_data_engine.py`:

    Example:

    Enter:

    ```bash
        import requests
        import time
        def create_data_engine_table(api_key,crn):
            headers = {
            'Authorization': 'Basic Yng6Yng=',
            }
            data = {
            'apikey': api_key,
            'grant_type': 'urn:ibm:params:oauth:grant-type:apikey',
            }
            response = requests.post('https://iam.cloud.ibm.com/identity/token', headers=headers, data=data)
            token = response.json()['access_token']

            headers_token = {
            'Accept': 'application/json',
            'Authorization': f"Bearer {token}",
            }
            params = {
            'instance_crn': crn,
            }
            json_data = {
            'statement': 'CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) USING PARQUET LOCATION cos://us-south/mybucket/countriescapitals.parquet',
            }
            response = requests.post('https://api.dataengine.cloud.ibm.com/v3/sql_jobs', params=params, headers=headers_token, json=json_data)
            job_id = response.json()['job_id']
            time.sleep(10)
            response = requests.get(f'https://api.dataengine.cloud.ibm.com/v3/sql_jobs/{job_id}', params=params, headers=headers_token)
            if(response.json()['status']=='completed'):
                print(response.json())
    ```
    {: codeblock}
    {: python}

    In the above example, the location (`cos://us-south/mybucket/countriescapitals.parquet`) of the COS bucket is considered as `us-south`, which is the regional bucket. If you are using any other region, select the corresponding alias from the [Data engine endpoints](#aeendpoints).

    

    The payload for the above application `create_table_data_engine_payload.json` also needs to provide the {{site.data.keyword.sqlquery_short}} credentials with the exact standard {{site.data.keyword.sqlquery_short}} alias, in this case: "us-south".

    Example:

    Enter:

    ```bash
        {
            "application_details": {
                "conf": {
                    "spark.hadoop.fs.cos.us-south.endpoint": "CHANGEME",
                    "spark.hadoop.fs.cos.us-south.access.key": "CHANGEME",
                    "spark.hadoop.fs.cos.us-south.secret.key": "CHANGEME"
                },
            "application": "cos://mybucket.us-south/create_table_data_engine.py",
            "arguments": ["<CHANGEME-CRN-DATA-ENGINE-INSTANCE>","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
            }
        }
    ```
    {: codeblock}
    {: json}

    Parameter values:

    In the above example, the regional COS bucket from `us-south`is considered. If you are using any other region, select the corresponding alias from the [Data engine endpoints](#aeendpoints).

    

    Make sure that you select the standard aliases.
    {: important}

## Reading data from table by passing full list of Data Engine parameters
{: #full-list-Data-Engine}

You can read the data from the metastore table using the SQL querry.

Read the data from the table by using the Spark SQL in the following application called `select_query_data_engine.py`:


```python
from pyspark.sql import SparkSession
import time

def init_spark():
spark = SparkSession.builder.appName("dataengine-table-select-test").getOrCreate()
sc = spark.sparkContext
return spark,sc

def select_query_data_engine(spark,sc):
tablesDF=spark.sql("SHOW TABLES")
tablesDF.show()
statesDF=spark.sql("SELECT * from COUNTRIESCAPITALS");
statesDF.show()

def main():
spark,sc = init_spark()
select_query_data_engine(spark,sc)

if __name__ == '__main__':
main()
```
{: codeblock}

Note that for the SELECT command to work, you must pass the {{site.data.keyword.cos_full_notm}} identifiers as one of the standard {{site.data.keyword.sqlquery_short}} aliases, in this example, we have used `us-south`. If you do not pass the expected ones, you might see the following error: `Configuration parse exception: Access KEY is empty. Please provide valid access key`.



`select_query_data_engine_payload.json`:

```json
{
        "application_details": {
            "conf": {
                "spark.hadoop.fs.cos.us-south.endpoint": "CHANGEME",
                "spark.hadoop.fs.cos.us-south.access.key": "CHANGEME",
                "spark.hadoop.fs.cos.us-south.secret.key": "CHANGEME",
                "spark.hive.metastore.truststore.password" : "changeit",
                "spark.hive.execution.engine":"spark",
                "spark.hive.metastore.client.plain.password":"APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE",
                "spark.hive.metastore.uris":"thrift://catalog.us.dataengine.cloud.ibm.com:9083",
                "spark.hive.metastore.client.auth.mode":"PLAIN",
                "spark.hive.metastore.use.SSL":"true",
                "spark.hive.stats.autogather":"false",
                "spark.hive.metastore.client.plain.username":"<CHANGEME-CRN-DATA-ENGINE-INSTANCE>",
                # for spark 3.4
                "spark.hive.metastore.truststore.path":"/opt/ibm/jdk/lib/security/cacerts",
                "spark.sql.catalogImplementation":"hive",
                "spark.sql.hive.metastore.jars":"/opt/ibm/connectors/data-engine/hms-client/*",
                "spark.sql.hive.metastore.version":"3.0",
                "spark.sql.warehouse.dir":"file:///tmp",
                "spark.sql.catalogImplementation":"hive",
                "spark.hadoop.metastore.catalog.default":"spark"
            },
            "application": "cos://mybucket.us-south/select_query_data_engine.py"
        }
    }
```
{: codeblock} 

In the above example, the regional COS bucket from `us-south`is considered. If you are using any other region, select the corresponding alias from the [Data engine endpoints](#aeendpoints).
Also, the metastore URL is provided as `thrift://catalog.us.dataengine.cloud.ibm.com:9083`. For more information about other applicable endpoints(thrift), see [Thrift endpoint](https://cloud.ibm.com/docs/sql-query?topic=sql-query-hive_metastore#hive_compatible_client).

Parameter value:

* CRN-DATA-ENGINE-INSTANCE: specify the crn of the data engine instance.

Make sure that you select the standard aliases.
{: important}

## Read Data from table using simpler Convenience API
{: #convenience-api}

If you want to do a quick test of the Hive metastore by specifying {{site.data.keyword.sqlquery_notm}} connection API in your application, you can use the convenience API shown in the following PySpark example.

In this example, there is no need to pass any {{site.data.keyword.sqlquery_notm}} Hive metastore parameters to your application. The call to `SparkSessionWithDataengine.enableDataengine` initializes the connections to {{site.data.keyword.sqlquery_notm}} without the additional {{site.data.keyword.sqlquery_notm}} Hive metastore parameters.

dataengine-job-convenience_api.py:

```python
from dataengine import SparkSessionWithDataengine
from pyspark.sql import SQLContext
import sys
 from pyspark.sql import SparkSession
import time

def dataengine_table_test(spark,sc):
  tablesDF=spark.sql("SHOW TABLES")
  tablesDF.show()
  statesDF=spark.sql("SELECT * from COUNTRIESCAPITALS");
  statesDF.show()

def main():
  if __name__ == '__main__':
    if len (sys.argv) < 2:
        exit(1)
    else:
        crn = sys.argv[1]
        apikey = sys.argv[2]
        session_builder = SparkSessionWithDataengine.enableDataengine(crn, apikey, "public", "/opt/ibm/connectors/data-engine/hms-client")
        spark = session_builder.appName("Spark DataEngine integration test").getOrCreate()
        sc = spark.sparkContext
        dataengine_table_test (spark,sc)

if __name__ == '__main__':
  main()
```
{: codeblock}

The following is the payload for the convenience `dataengine-job-convenience_api.py`:

Example:

Enter:

```json
{
    "application_details": {
        "conf": {
            "spark.hadoop.fs.cos.us-south.endpoint": "CHANGEME",
            "spark.hadoop.fs.cos.us-south.access.key": "CHANGEME",
            "spark.hadoop.fs.cos.us-south.secret.key": "CHANGEME"
        }
        "application": "cos://mybucket.us-south/dataengine-job-convenience_api.py",
        "arguments": ["<CHANGEME-CRN-DATA-ENGINE-INSTANCE>","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
    }
}
```
{: codeblock}

Parameter values:
In the above example, the regional COS bucket from `us-south`is considered. If you are using any other region, select the corresponding alias from the [Data engine endpoints](#aeendpoints).


Make sure that you select the standard aliases.
{: important}




### Cloud {{site.data.keyword.cos_short}} endpoints
{: #aeendpoints}

Your Cloud {{site.data.keyword.cos_short}} instance has one of the supported endpoints. {{site.data.keyword.sqlquery_short}} supports all [public and private {{site.data.keyword.cos_short}} endpoints](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-endpoints). To save space, you can use the alias that is shown instead of the full endpoint name.

Aliases to tethering endpoints (specific endpoints within cross region domains, for example, `dal-us-geo`) are considered legacy. They continue to work until further notice but are planned to be deprecated sometime in the future. To be prepared, update your applications to use the alias of the corresponding cross region endpoint (for example, `us-geo`).

{{site.data.keyword.sqlquery_short}} always uses the internal endpoint to interact with {{site.data.keyword.cos_short}}, even if an external endpoint was specified in the query. The result location for a query always indicates the external endpoint name. When you interact with {{site.data.keyword.sqlquery_short}} programmatically through the API, you can use the internal endpoint name to read results instead of the external endpoint name that is returned by the API.
{: note}

The following tables list some examples of currently supported {{site.data.keyword.sqlquery_short}} endpoints.

|Cross region endpoint name | Alias|
|--- | ---|
|`s3.us.cloud-object-storage.appdomain.cloud` | `us-geo`|
|`s3.eu.cloud-object-storage.appdomain.cloud` | `eu-geo`|
|`s3.ap.cloud-object-storage.appdomain.cloud` | `ap-geo`|
{: caption="Cross region endpoints" caption-side="bottom"}

|Regional endpoint name | Alias|
|--- | ---|
|`s3.eu-de.cloud-object-storage.appdomain.cloud` | `eu-de`|
|`s3.eu-gb.cloud-object-storage.appdomain.cloud` | `eu-gb`|
|`s3.us-south.cloud-object-storage.appdomain.cloud | `us-south`|
{: caption="Regional endpoints" caption-side="bottom"}
