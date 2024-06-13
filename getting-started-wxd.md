---

copyright:
  years: 2017, 2023
lastupdated: "2023-07-07"

keywords: watsonx.data, spark, table, maintenance
subcollection: AnalyticsEngine

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Spark use case
{: #aerun_samp_file}

This topic provides the procedure to run Spark use cases for {{site.data.keyword.lakehouse_short}} by using Python samples. All the samples are written by using Spark Python APIs.

## Prerequisites
{: #aespk_preq}

* Provision an {{site.data.keyword.lakehouse_full}} instance.
* Configure an {{site.data.keyword.iae_full_notm}} instance.
* Cloud Object Storage bucket connection details.

## About the sample use case
{: #aeabt_samp_usecase}

The sample file demonstrates the following functionalities:

* Accessing tables from {{site.data.keyword.lakehouse_short}}

    The **Create a database in Lakehouse catalog** section from the [sample python file](#python_file) creates a database **demodb** in the configured {{site.data.keyword.lakehouse_short}} instance with a catalog named **lakehouse**. **demodb** is configured to store all the data and metadata under the Cloud Object Storage(COS) bucket **lakehouse-bucket**. It also creates an iceberg table **testTable** and accesses it.

* Ingesting data to {{site.data.keyword.lakehouse_short}}

    The **Ingest parquet data into a lakehouse table** section from the [sample python file](#python_file) allows you to ingest data in parquet and CSV format from a source Cloud Object Storage bucket **source-bucket** into a {{site.data.keyword.lakehouse_short}} table. Sample data in parquet format is inserted from source COS bucket **source-bucket** into the {{site.data.keyword.lakehouse_short}} table **yellow_taxi_2022** (see the [steps](#insert_samp_usecase-1) for inserting sample data into the source COS bucket). It also shows ingesting data in CSV format from COS bucket **source-bucket** into the table **zipcode** in the database **demodb**.

* Modifying schema in {{site.data.keyword.lakehouse_short}}

    The **Schema evolution** section from the [sample python file](#python_file) allows you to modify data in {{site.data.keyword.lakehouse_short}}.

* Performing table maintenance activities in {{site.data.keyword.lakehouse_short}}

    Table maintenance helps in keeping the {{site.data.keyword.lakehouse_short}} table performant. Iceberg provides table maintenance procedures out of the box that allows performing powerful table optimizations in a declarative fashion. The sample below demonstrates how to do some table maintenance operations by using Spark. For more information about the Iceberg Spark table maintenance operations, see [Table Operations](https://iceberg.apache.org/docs/1.2.1/spark-procedures/).

## Running the sample use case
{: #aeabt_samp_run}

Follow the steps to run the Spark sample python file.

### Spark sample python file
{: #aepython_file}

```bash

from pyspark.sql import SparkSession
import os

def init_spark():
  spark = SparkSession.builder \
    .appName("lh-hms-cloud") \
    .config("spark.hadoop.fs.s3a.bucket.lakehouse-bucket.endpoint" ,"s3.direct.us-south.cloud-object-storage.appdomain.cloud") \
    .config("spark.hadoop.fs.s3a.bucket.lakehouse-bucket.access.key" ,"<lakehouse-bucket-access-key>") \
    .config("spark.hadoop.fs.s3a.bucket.lakehouse-bucket.secret.key" ,"<lakehouse-bucket-secret-key>") \
    .config("spark.hadoop.fs.s3a.bucket.source-bucket.endpoint" ,"s3.direct.us-south.cloud-object-storage.appdomain.cloud") \
    .config("spark.hadoop.fs.s3a.bucket.source-bucket.access.key" ,"<source-bucket-access-key>") \
    .config("spark.hadoop.fs.s3a.bucket.source-bucket.secret.key" ,"<source-bucket-secret-key>") \
    .enableHiveSupport() \
    .getOrCreate()
  return spark

def create_database(spark):
    # Create a database in the lakehouse catalog
    spark.sql("create database if not exists lakehouse.demodb LOCATION 's3a://lakehouse-bucket/'")

def list_databases(spark):
    # list the database under lakehouse catalog
    spark.sql("show databases from lakehouse").show()

def basic_iceberg_table_operations(spark):
    # demonstration: Create a basic Iceberg table, insert some data and then query table
    spark.sql("create table if not exists lakehouse.demodb.testTable(id INTEGER, name VARCHAR(10), age INTEGER, salary DECIMAL(10, 2)) using iceberg").show()
    spark.sql("insert into lakehouse.demodb.testTable values(1,'Alan',23,3400.00),(2,'Ben',30,5500.00),(3,'Chen',35,6500.00)")
    spark.sql("select * from lakehouse.demodb.testTable").show()

def create_table_from_parquet_data(spark):
    # load parquet data into dataframce
    df = spark.read.option("header",True).parquet("s3a://source-bucket/nyc-taxi/yellow_tripdata_2022-01.parquet")
    # write the dataframe into an Iceberg table
    df.writeTo("lakehouse.demodb.yellow_taxi_2022").create()
    # describe the table created
    spark.sql('describe table lakehouse.demodb.yellow_taxi_2022').show(25)
    # query the table
    spark.sql('select * from lakehouse.demodb.yellow_taxi_2022').count()

def ingest_from_csv_temp_table(spark):
    # load csv data into a dataframe
    csvDF = spark.read.option("header",True).csv("s3a://source-bucket/zipcodes.csv")
    csvDF.createOrReplaceTempView("tempCSVTable")
    # load temporary table into an Iceberg table
    spark.sql('create or replace table lakehouse.demodb.zipcodes using iceberg as select * from tempCSVTable')
    # describe the table created
    spark.sql('describe table lakehouse.demodb.zipcodes').show(25)
    # query the table
    spark.sql('select * from lakehouse.demodb.zipcodes').show()

def ingest_monthly_data(spark):
    df_feb = spark.read.option("header",True).parquet("s3a://source-bucket//nyc-taxi/yellow_tripdata_2022-02.parquet")
    df_march = spark.read.option("header",True).parquet("s3a://source-bucket//nyc-taxi/yellow_tripdata_2022-03.parquet")
    df_april = spark.read.option("header",True).parquet("s3a://source-bucket//nyc-taxi/yellow_tripdata_2022-04.parquet")
    df_may = spark.read.option("header",True).parquet("s3a://source-bucket//nyc-taxi/yellow_tripdata_2022-05.parquet")
    df_june = spark.read.option("header",True).parquet("s3a://source-bucket//nyc-taxi/yellow_tripdata_2022-06.parquet")

    df_q1_q2 = df_feb.union(df_march).union(df_april).union(df_may).union(df_june)
    df_q1_q2.write.insertInto("lakehouse.demodb.yellow_taxi_2022")

def perform_table_maintenance_operations(spark):
    # Query the metadata files table to list underlying data files
    spark.sql("SELECT file_path, file_size_in_bytes FROM lakehouse.demodb.yellow_taxi_2022.files").show()

    # There are many smaller files compact them into files of 200MB each using the
    # `rewrite_data_files` Iceberg Spark procedure
    spark.sql(f"CALL lakehouse.system.rewrite_data_files(table => 'demodb.yellow_taxi_2022', options => map('target-file-size-bytes','209715200'))").show()

    # Again, query the metadata files table to list underlying data files; 6 files are compacted
    # to 3 files
    spark.sql("SELECT file_path, file_size_in_bytes FROM lakehouse.demodb.yellow_taxi_2022.files").show()

    # List all the snapshots
    # Expire earlier snapshots. Only latest one with comacted data is required
    # Again, List all the snapshots to see only 1 left
    spark.sql("SELECT committed_at, snapshot_id, operation FROM lakehouse.demodb.yellow_taxi_2022.snapshots").show()
    #retain only the latest one
    latest_snapshot_committed_at = spark.sql("SELECT committed_at, snapshot_id, operation FROM lakehouse.demodb.yellow_taxi_2022.snapshots").tail(1)[0].committed_at
    print (latest_snapshot_committed_at)
    spark.sql(f"CALL lakehouse.system.expire_snapshots(table => 'demodb.yellow_taxi_2022',older_than => TIMESTAMP '{latest_snapshot_committed_at}',retain_last => 1)").show()
    spark.sql("SELECT committed_at, snapshot_id, operation FROM lakehouse.demodb.yellow_taxi_2022.snapshots").show()

    # Removing Orphan data files
    spark.sql(f"CALL lakehouse.system.remove_orphan_files(table => 'demodb.yellow_taxi_2022')").show(truncate=False)

    # Rewriting Manifest Files
    spark.sql(f"CALL lakehouse.system.rewrite_manifests('demodb.yellow_taxi_2022')").show()


def evolve_schema(spark):
    # demonstration: Schema evolution
    # Add column fare_per_mile to the table
    spark.sql('ALTER TABLE lakehouse.demodb.yellow_taxi_2022 ADD COLUMN(fare_per_mile double)')
    # describe the table
    spark.sql('describe table lakehouse.demodb.yellow_taxi_2022').show(25)


def clean_database(spark):
    # clean-up the demo database
    spark.sql('drop table if exists lakehouse.demodb.testTable purge')
    spark.sql('drop table if exists lakehouse.demodb.zipcodes purge')
    spark.sql('drop table if exists lakehouse.demodb.yellow_taxi_2022 purge')
    spark.sql('drop database if exists lakehouse.demodb cascade')

def main():
    try:
        spark = init_spark()

        create_database(spark)
        list_databases(spark)

        basic_iceberg_table_operations(spark)

        # demonstration: Ingest parquet and csv data into a wastonx.data Iceberg table
        create_table_from_parquet_data(spark)
        ingest_from_csv_temp_table(spark)

        # load data for the month of Feburary to June into the table yellow_taxi_2022 created above
        ingest_monthly_data(spark)

        # demonstration: Table maintenance
        perform_table_maintenance_operations(spark)

        # demonstration: Schema evolution
        evolve_schema(spark)
    finally:
        # clean-up the demo database
        clean_database(spark)
        spark.stop()

if __name__ == '__main__':
  main()
```
{: codeblock}

1. Save the following sample python file.
1. Upload the Python file to the Cloud Object Storage bucket. You must maintain the Spark applications and their dependencies in a Cloud Object Storage bucket and not mix them with data buckets.
1. Generate the IAM token for the {{site.data.keyword.iae_full_notm}} token. For more information about how to generate an IAM token, see [IAM token](https://test.cloud.ibm.com/docs/watsonxdata?topic=watsonxdata-con-presto-thru-cli#get-api-iam-token)
1. Run the following curl command to submit the Spark application:

   ```bash
    curl https://api.<region>.ae.cloud.ibm.com/v3/analytics_engines/<iae-instance-guid>/spark_applications -H "Authorization: Bearer <iam-bearer-token>" -X POST -d '{
    "application_details": {
        "application": "s3a://<application_bucket>/lakehouse-hms-test-cloud-doc-sample.py",
        "conf": {
            "spark.hadoop.fs.s3a.bucket.<application-bucket>.endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
            "spark.hadoop.fs.s3a.bucket.<application-bucket>.access.key": "<hmac_access_key_for_application-bucket>",
            "spark.hadoop.fs.s3a.bucket.<application-bucket>.secret.key": "<hmac_secret_key_for_application-bucket>"
        }
    }
    }'
   ```
   {: codeblock}


This sample is tested on the Cloud Object Storage buckets in the **us-south** region. Change the region in the Cloud Object Storage endpoint configuration as per the region where your Cloud Object Storage buckets reside. It is recommended to provision the COS buckets in the region where {{site.data.keyword.iae_short}} instance is provisioned.
{: note}

## Inserting sample data into the COS bucket
{: #insert_samp_usecase-1}

To insert data to COS, follow the steps below.

1. Create the COS bucket.

    ```bash
    ibmcloud plugin install cloud-object-storage
    ```
    {: codeblock}


    As a user of Object Storage, you not only need to know the API key or the HMAC keys to configure Object Storage, but also the IBM Analytics Engine service endpoints to connect to Object Storage. See [Selecting regions and endpoints](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-endpoints) for more information on the endpoints to use based on your Object Storage bucket type, such as regional versus cross-regional. You can also view the endpoints across regions for your Object Storage service by selecting the service on your IBM Cloud dashboard and clicking **Endpoint** in the navigation pane. Always choose the **direct endpoint**. Direct endpoint provide better performance and do not incur charges. An example of an endpoint for US-South Cross region is `s3.direct.us.cloud-object-storage.appdomain.cloud`.

2.  Download first six months taxi data sample for the year 2022. Choose the required data format and download from the corresponding locations:

    * [Sample parquet file](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).
    * [Sample CSV file](https://raw.githubusercontent.com/spark-examples/spark-scala-examples/3ea16e4c6c1614609c2bd7ebdffcee01c0fe6017/src/main/resources/zipcodes.csv)

3. Use the COS cli to upload the sample data into COS bucket.

    ```bash
    ibmcloud cos upload --bucket test-cos-storage-bucket --key <your sample data file name> --file <your sample data file name>
    ```
    {: codeblock}
