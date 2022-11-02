---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-30"

subcollection: analyticsengine

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


1. Create an {{site.data.keyword.sqlquery_notm}} instance using the Standard plan. See [{{site.data.keyword.sqlquery_short}}](http://cloud.ibm.com/catalog/services/data-engine-previously-sql-query).

    After you have provisioned the {{site.data.keyword.sqlquery_short}} instance:
    1. Make a note of the CRN of the instance.
    1. Create an account level API key or service ID level API key with access to the instance.
    1. This service ID should be granted access to both the {{site.data.keyword.sqlquery_short}} instance as well as the {{site.data.keyword.cos_full_notm}} bucket.

    You can then configure your {{site.data.keyword.iae_full_notm}} instance to use the default metastore configuration either at instance level or at application level as needed.

1. Specify the {{site.data.keyword.sqlquery_short}} metastore connection parameters. The following parameters are the additional {{site.data.keyword.sqlquery_short}} metastore parameters that you should pass as part of the Spark application payload or specify as instance defaults if you want to access metastore data across all applications:

    ```sh
    "spark.hive.metastore.truststore.password" : "changeit",
    "spark.hive.execution.engine":"spark",
    "spark.hive.metastore.client.plain.password":"<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>",
    "spark.hive.metastore.uris":"CHANGEME-thrift://catalog.us.dataengine.cloud.ibm.com:9083-CHANGEME",
    "spark.hive.metastore.client.auth.mode":"PLAIN",
    "spark.hive.metastore.use.SSL":"true",
    "spark.hive.stats.autogather":"false",
    "spark.hive.metastore.client.plain.username":"CHANGEME-crn:v1:bluemix:public:sql-query:us-south:a/abcdefgh::CHANGEME"
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

1. Generate and store data. Run the following regular PySpark application, called `generate-and-store-data.py` in this example, which stores Parquet data in some location on {{site.data.keyword.cos_full_notm}}. 

    ```python
    from pyspark.sql import SparkSession

    def init_spark():
        spark = SparkSession.builder.appName("dataengine-generate-store-parquet-data").getOrCreate()
        sc = spark.sparkContext
        return spark,sc

    def generate_and_store_data(spark):
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

1. Create the table schema definition. Note that you can't use standard Spark SQL syntax to create tables when using {{site.data.keyword.sqlquery_short}} as a metastore. There are two ways that you can use to create a table:

    - From the {{site.data.keyword.sqlquery_short}} user interface or by using the standard {{site.data.keyword.sqlquery_short}} API (see [Data Engine service REST V3 API](https://cloud.ibm.com/apidocs/sql-query-v3#introduction)) or Python SDK (see [ibmcloudsql](https://pypi.org/project/ibmcloudsql/)).

        ```sql
        CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) 
        USING PARQUET 
        LOCATION cos://us-geo/mybucket/countriescapitals.parquet
        ```
        {: codeblock}

    - Programmatically from within your PySpark application by using the following code snippet for PySpark called `create_table_data_engine.py`:

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
            'statement': 'CREATE TABLE COUNTRIESCAPITALS (Country string,Capital string) USING PARQUET LOCATION cos://us-geo/mybucket/countriescapitals.parquet',
            }
            response = requests.post('https://api.dataengine.cloud.ibm.com/v3/sql_jobs', params=params, headers=headers_token, json=json_data)
            job_id = response.json()['job_id']
            time.sleep(10)
            response = requests.get(f'https://api.dataengine.cloud.ibm.com/v3/sql_jobs/{job_id}', params=params, headers=headers_token)
            if(response.json()['status']=='completed'):
                print(response.json())    
        ```
        {: codeblock}

        Note that for the location URI (`cos://us-geo/mybucket/countriescapitals.parquet`) you need to pass one of the standard {{site.data.keyword.sqlquery_notm}} aliases. See [Endpoints](/docs/sql-query?topic=sql-query-overview#endpoints)

        The payload for the above application `create_table_data_engine_payload.json` also needs to provide the {{site.data.keyword.sqlquery_short}} credentials with the exact standard {{site.data.keyword.sqlquery_short}} alais, in this case: "us-geo"

        ```json
        {
            "application_details": {
                "conf": {
                    "spark.hadoop.fs.cos.us-geo.endpoint": "CHANGEME",
                    "spark.hadoop.fs.cos.us-geo.access.key": "CHANGEME",
                    "spark.hadoop.fs.cos.us-geo.secret.key": "CHANGEME"
                },
            "application": "cos://mybucket.us-geo/create_table_data_engine.py",
            "arguments": ["crn:v1:bluemix:public:sql-query:us-south:a/<CRN-DATA-ENGINE-INSTANCE>::","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
            }
        }
        ```
        {: codeblock}

1. Read the data from the table using the Spark SQL in the following application called `select_query_data_engine.py`:

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

    Note that for the SELECT command to work, you have to pass the {{site.data.keyword.cos_full_notm}} identifiers as one of the standard {{site.data.keyword.sqlquery_short}} aliases, in this example, you have to we have used `us-geo`. If you do not pass the expected ones, you might see the following error: `Configuration parse exception: Access KEY is empty. Please provide valid access key`.

    `select_query_data_engine_payload.json`:

    ```json
    {
        "application_details": {
            "conf": {
                "spark.hadoop.fs.cos.us-geo.endpoint": "CHANGEME",
                "spark.hadoop.fs.cos.us-geo.access.key": "CHANGEME",
                "spark.hadoop.fs.cos.us-geo.secret.key": "CHANGEME",
                "spark.hive.metastore.truststore.password" : "changeit",
                "spark.hive.execution.engine":"spark",
                "spark.hive.metastore.client.plain.password":"APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE",
                "spark.hive.metastore.uris":"thrift://catalog.us.dataengine.cloud.ibm.com:9083",
                "spark.hive.metastore.client.auth.mode":"PLAIN",
                "spark.hive.metastore.use.SSL":"true",
                "spark.hive.stats.autogather":"false",
                "spark.hive.metastore.client.plain.username":"crn:v1:bluemix:public:sql-query:us-south:a/<CRN-DATA-ENGINE-INSTANCE>::",
                "spark.hive.metastore.truststore.path":"file:///opt/ibm/jdk/jre/lib/security/cacerts",
                "spark.sql.catalogImplementation":"hive",
                "spark.sql.hive.metastore.jars":"/opt/ibm/connectors/data-engine/hms-client/*",
                "spark.sql.hive.metastore.version":"3.0",
                "spark.sql.warehouse.dir":"file:///tmp",
                "spark.sql.catalogImplementation":"hive",
                "spark.hadoop.metastore.catalog.default":"spark"
            },
            "application": "cos://mybucket.us-geo/select_query_data_engine.py"
        }
    }
    ```
    {: codeblock} 

## Convenience API
{: #convenience-api}

If you want to do a quick test of the Hive metastore by specifying {{site.data.keyword.sqlquery_notm}} connection API in your application, you can use the convenience API shown in the following PySpark example. 

In this example, there is no need to pass any {{site.data.keyword.sqlquery_notm}} Hive metastore parameters to your application. The call to `SparkSessionWithDataengine.enableDataengine` will initialize the connections to {{site.data.keyword.sqlquery_notm}} without the additional {{site.data.keyword.sqlquery_notm}} Hive metastore parameters.

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
```json
{
    "application_details": {
        "conf": {
            "spark.hadoop.fs.cos.us-geo.endpoint": "CHANGEME",
            "spark.hadoop.fs.cos.us-geo.access.key": "CHANGEME",
            "spark.hadoop.fs.cos.us-geo.secret.key": "CHANGEME"
        }
        "application": "cos://mybucket.us-geo/dataengine-job-convenience_api.py",
        "arguments": ["crn:v1:bluemix:public:sql-query:us-south:a/<CRN-DATA-ENGINE-INSTANCE>::","<APIKEY-WITH-ACCESS-TO-DATA-ENGINE-INSTANCE>"]
    }
}
```
{: codeblock}










