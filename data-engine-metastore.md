---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
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
• For more information on the currently supported data engine endpoints, see [Data engine endpoints](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview#endpoints)
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

        ```sql
        CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) 
        USING PARQUET 
        LOCATION cos://ALIAS NAME/mybucket/countriescapitals.parquet
        ```
        {: codeblock}
        {: sql}

    Parameter values:
    ALIAS NAME: The data engine endpoint for your region. For more information on the currently supported data engine endpoints, see [Data engine endpoints](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview#endpoints). Make sure that you select the standard aliases.

- Programmatically from within your PySpark application by using the following code snippet for PySpark called `create_table_data_engine.py`:

    Example:

    Enter:

        ```python
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
            'statement': 'CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) USING PARQUET LOCATION cos://ALIAS NAME/mybucket/countriescapitals.parquet',
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

    Parameter values:
    ALIAS NAME: Note that for the location URI (`cos://ALIAS NAME/mybucket/countriescapitals.parquet`) you need to pass one of the standard {{site.data.keyword.sqlquery_notm}} aliases. See [Data engine endpoints](/docs/sql-query?topic=sql-query-overview#endpoints)

    The payload for the above application `create_table_data_engine_payload.json` also needs to provide the {{site.data.keyword.sqlquery_short}} credentials with the exact standard {{site.data.keyword.sqlquery_short}} alais, in this case: "ALIAS NAME"

    Example:

    Enter:

        ```json
        {
            "application_details": {
                "conf": {
                    "spark.hadoop.fs.cos.ALIAS NAME.endpoint": "CHANGEME",
                    "spark.hadoop.fs.cos.ALIAS NAME.access.key": "CHANGEME",
                    "spark.hadoop.fs.cos.ALIAS NAME.secret.key": "CHANGEME"
                },
            "application": "cos://mybucket.ALIAS NAME/create_table_data_engine.py",
            "arguments": ["<CHANGEME-CRN-DATA-ENGINE-INSTANCE>","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
            }
        }
        ```
        {: codeblock}
        {: json}

    Parameter values:

    ALIAS NAME: specify the data engine endpoint for your region. For more information on the currently supported data engine endpoints, see [Data engine endpoints](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview#endpoints).

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

Note that for the SELECT command to work, you must pass the {{site.data.keyword.cos_full_notm}} identifiers as one of the standard {{site.data.keyword.sqlquery_short}} aliases, in this example, we have used `ALIAS NAME`. If you do not pass the expected ones, you might see the following error: `Configuration parse exception: Access KEY is empty. Please provide valid access key`.



`select_query_data_engine_payload.json`:

```json
{
        "application_details": {
            "conf": {
                "spark.hadoop.fs.cos.ALIAS NAME.endpoint": "CHANGEME",
                "spark.hadoop.fs.cos.ALIAS NAME.access.key": "CHANGEME",
                "spark.hadoop.fs.cos.ALIAS NAME.secret.key": "CHANGEME",
                "spark.hive.metastore.truststore.password" : "changeit",
                "spark.hive.execution.engine":"spark",
                "spark.hive.metastore.client.plain.password":"APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE",
                "spark.hive.metastore.uris":"THRIFT URL",
                "spark.hive.metastore.client.auth.mode":"PLAIN",
                "spark.hive.metastore.use.SSL":"true",
                "spark.hive.stats.autogather":"false",
                "spark.hive.metastore.client.plain.username":"<CHANGEME-CRN-DATA-ENGINE-INSTANCE>",
                # for spark 3.3
                "spark.hive.metastore.truststore.path":"/opt/ibm/jdk/lib/security/cacerts",
                # for spark 3.1, spark 3.2
                "spark.hive.metastore.truststore.path":"file:///opt/ibm/jdk/jre/lib/security/cacerts",
                "spark.sql.catalogImplementation":"hive",
                "spark.sql.hive.metastore.jars":"/opt/ibm/connectors/data-engine/hms-client/*",
                "spark.sql.hive.metastore.version":"3.0",
                "spark.sql.warehouse.dir":"file:///tmp",
                "spark.sql.catalogImplementation":"hive",
                "spark.hadoop.metastore.catalog.default":"spark"
            },
            "application": "cos://mybucket.ALIAS NAME/select_query_data_engine.py"
        }
    }
```
{: codeblock} 

Parameter values:

* ALIAS NAME: specify the data engine endpoint for your region. For more information on the currently supported data engine endpoints, see [Data engine endpoints](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview#endpoints).

* THRIFT URL: specify the region-specific thrift URL. For example, thrift://catalog.us.dataengine.cloud.ibm.com:9083. For more information about the applicable endpoints(thrift), see [Thrift endpoint](https://cloud.ibm.com/docs/sql-query?topic=sql-query-hive_metastore#hive_compatible_client).

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
            "spark.hadoop.fs.cos.ALIAS NAME.endpoint": "CHANGEME",
            "spark.hadoop.fs.cos.ALIAS NAME.access.key": "CHANGEME",
            "spark.hadoop.fs.cos.ALIAS NAME.secret.key": "CHANGEME"
        }
        "application": "cos://mybucket.ALIAS NAME/dataengine-job-convenience_api.py",
        "arguments": ["<CHANGEME-CRN-DATA-ENGINE-INSTANCE>","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
    }
}
```
{: codeblock}

Parameter values:

ALIAS NAME: specify the data engine endpoint for your region. For more information on the currently supported data engine endpoints, see [Data engine endpoints](https://cloud.ibm.com/docs/sql-query?topic=sql-query-overview#endpoints).

Make sure that you select the standard aliases.
{: important}


<!-- 1. Specify the {{site.data.keyword.sqlquery_short}} metastore connection parameters. The following parameters are the additional {{site.data.keyword.sqlquery_short}} metastore parameters that you should pass as part of the Spark application payload or specify as instance defaults if you want to access metastore data across all applications:

    ```sh
    "spark.hive.metastore.truststore.password" : "changeit",
    "spark.hive.execution.engine":"spark",
    "spark.hive.metastore.client.plain.password":"<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>",
    "spark.hive.metastore.uris":"CHANGE-ME-REGION-SPECIFIC-THRIFT-URL",
    "spark.hive.metastore.client.auth.mode":"PLAIN",
    "spark.hive.metastore.use.SSL":"true",
    "spark.hive.stats.autogather":"false",
    "spark.hive.metastore.client.plain.username":"CHANGE-ME-INSTANCE-CRN",
    # for spark 3.3


    "spark.hive.metastore.truststore.path":"file:///opt/ibm/jdk/jre/lib/security/cacerts",
    "spark.sql.catalogImplementation":"hive",
    "spark.sql.hive.metastore.jars":"/opt/ibm/connectors/data-engine/hms-client/*",
    "spark.sql.hive.metastore.version":"3.0",
    "spark.sql.warehouse.dir":"file:///tmp",
    "spark.sql.catalogImplementation":"hive",
    "spark.hadoop.metastore.catalog.default":"spark"
    ```
    {: codeblock}

    For the variables:
    - CHANGEME-thrift: use the thrift endpoint for your region. For valid values, see [Connecting Apache Spark with Data Engine](/docs/sql-query?topic=sql-query-hive_metastore#external_usage).
    - CHANGEME-crn: pick the CRN from the {{site.data.keyword.sqlquery_short}} service instance details
    - REGION: specify your region code. For example, 'us-south'. -->
