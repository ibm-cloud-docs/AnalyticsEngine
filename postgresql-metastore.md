---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-30"

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

# Using {{site.data.keyword.databases-for-postgresql_full_notm}} as external metastore 
{: #postgresql-external-metastore}

You can use {{site.data.keyword.databases-for-postgresql_full_notm}} to externalize metadata outside the {{site.data.keyword.iae_full_notm}} Spark cluster.

1. Create an {{site.data.keyword.databases-for-postgresql_full_notm}} instance. See [{{site.data.keyword.databases-for-postgresql}}](https://cloud.ibm.com/databases/databases-for-postgresql/create).

    Choose the configurations based on your requirements. Make sure to choose **Both public & private network** for the endpoint configuration. After you have created the instance and the service instance credentials, make a note of the database name, port, user name, password and certificate.
1. Upload the {{site.data.keyword.databases-for-postgresql}} certificate to an {{site.data.keyword.cos_full_notm}} bucket where you are maintaining your application code.

    To access {{site.data.keyword.databases-for-postgresql}}, you need to provide a client certificate. Get the Base64 decoded certificate from the service credentials of the {{site.data.keyword.databases-for-postgresql}} instance and upload the file (name it say, `postgres.cert`) to a {{site.data.keyword.cos_short}} bucket in a specific IBM Cloud location. Later you will need to download this certificate and make it available in the {{site.data.keyword.iae_full_notm}} instance Spark workloads for connecting to the metastore
1. Customize the {{site.data.keyword.iae_full_notm}} instance to include the {{site.data.keyword.databases-for-postgresql}} certificate. See [Script based customization](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-script).

    This step customizes the {{site.data.keyword.iae_full_notm}} instance to make the {{site.data.keyword.databases-for-postgresql}} certificate available to all Spark workloads run against the instance through the library set.

    1. Upload the `customization_script.py` from the page in [Script based customization](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-script) to a {{site.data.keyword.cos_full_notm}} bucket.
    1. Run `postgres-cert-customization-submit.json` that uses the spark-submit REST API to customize the instance. Note that the code references `postgres.cert` that you uploaded to {{site.data.keyword.cos_full_notm}}.

        ```json
        {
            "application_details": 
            {
               "application": "/opt/ibm/customization-scripts/customize_instance_app.py",
            "arguments": ["{\"library_set\":{\"action\":\"add\",\"name\":\"certificate_library_set\",\"script\":{\"source\":\"py_files\",\"params\":[\"https://s3.direct.<CHANGME>.cloud-object-storage.appdomain.cloud\",\"<CHANGEME_BUCKET_NAME>\",\"postgres.cert\",\"<CHANGEME_ACCESS_KEY>\",\"<CHANGEME_SECRET_KEY>\"]}}}"],
            "py-files": "cos://CHANGEME_BUCKET_NAME.mycosservice/customization_script.py"
            }
        } 
        ```
        {: codeblock}

        Note that the library set name `certificate_library_set` must match the value of the {{site.data.keyword.databases-for-postgresql}} metastore connection parameter `ae.spark.librarysets` that you specified.
1. Specify the following {{site.data.keyword.databases-for-postgresql}} metastore connection parameters as part of the Spark application payload or as instance defaults. Make sure that you use the private endpoint for the `"spark.hadoop.javax.jdo.option.ConnectionURL"` parameter below:

    ```sh
    "spark.hadoop.javax.jdo.option.ConnectionDriverName": "org.postgresql.Driver",
    "spark.hadoop.javax.jdo.option.ConnectionUserName": "ibm_cloud_<CHANGEME>",
    "spark.hadoop.javax.jdo.option.ConnectionPassword": "<CHANGEME>",
    "spark.sql.catalogImplementation": "hive",
    "spark.hadoop.hive.metastore.schema.verification": "false",
    "spark.hadoop.hive.metastore.schema.verification.record.version": "false",
    "spark.hadoop.datanucleus.schema.autoCreateTables":"true",
    "spark.hadoop.javax.jdo.option.ConnectionURL": "jdbc:postgresql://<CHANGEME>.databases.appdomain.CHANGEME/ibmclouddb?sslmode=verify-ca&sslrootcert=/home/spark/shared/user-libs/certificate_library_set/custom/postgres.cert&socketTimeout=30",
    "ae.spark.librarysets":"certificate_library_set"
    ```
    {: codeblock}

1. Set up the Hive metastore schema in the {{site.data.keyword.databases-for-postgresql}} instance because there are no tables in the public schema of {{site.data.keyword.databases-for-postgresql}} database when you create the instance. This step executes the Hive schema related DDL so that metastore data can be stored in them. After running the following Spark application called `postgres-create-schema.py`, you will see the Hive metadata tables created against the "public" schema of the instance.

    ```python
    from pyspark.sql import SparkSession
    import time
    def init_spark():
      spark = SparkSession.builder.appName("postgres-create-schema").getOrCreate()
      sc = spark.sparkContext
      return spark,sc
    def create_schema(spark,sc):
      tablesDF=spark.sql("SHOW TABLES")
      tablesDF.show()
      time.sleep(30)
    def main():
      spark,sc = init_spark()
      create_schema(spark,sc)
    if __name__ == '__main__':
      main()
    ```
    {: codeblock}

1. Now run the following script called `postgres-parquet-table-create.py` to create a Parquet table with metadata from {{site.data.keyword.cos_full_notm}} in the {{site.data.keyword.databases-for-postgresql}} database.

    ```python
    from pyspark.sql import SparkSession
    import time
    def init_spark():
      spark = SparkSession.builder.appName("postgres-create-parquet-table-test").getOrCreate()
      sc = spark.sparkContext
      return spark,sc
    def generate_and_store_data(spark,sc):
      data =[("1","Romania","Bucharest","81"),("2","France","Paris","78"),("3","Lithuania","Vilnius","60"),("4","Sweden","Stockholm","58"),("5","Switzerland","Bern","51")]
      columns=["Ranking","Country","Capital","BroadBandSpeed"]
      df=spark.createDataFrame(data,columns)
      df.write.parquet("cos://<CHANGEME-BUCKET>.mycosservice/broadbandspeed")
    def create_table_from_data(spark,sc):
      spark.sql("CREATE TABLE MYPARQUETBBSPEED (Ranking STRING, Country STRING, Capital STRING, BroadBandSpeed STRING) STORED AS PARQUET  location 'cos://CHANGEME-BUCKET.mycosservice/broadbandspeed/'")
      df2=spark.sql("SELECT * from MYPARQUETBBSPEED")
      df2.show()
    def main():
      spark,sc = init_spark()
      generate_and_store_data(spark,sc)
      create_table_from_data(spark,sc)
      time.sleep(30)
    if __name__ == '__main__':
      main()
    ```
    {: codeblock}

1. Run the following PySpark script called `postgres-parquet-table-select.py` to access this Parquet table with metadata from another Spark workload:

    ```python
    from pyspark.sql import SparkSession
    import time
    def init_spark():
      spark = SparkSession.builder.appName("postgres-select-parquet-table-test").getOrCreate()
      sc = spark.sparkContext
      return spark,sc
    def select_data_from_table(spark,sc):
      df=spark.sql("SELECT * from MYPARQUETBBSPEED")
      df.show()
    def main():
      spark,sc = init_spark()
      select_data_from_table(spark,sc)
      time.sleep(60)
    if __name__ == '__main__':
     main()
    ```
    {: codeblock}
