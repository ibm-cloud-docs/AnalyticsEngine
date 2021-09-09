---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Configuring Parquet encryption on Apache Hive tables
{: #parquet-encryption-on-hive-tables}

Spark supports reading and writing data stored in Hive tables. If you want to encrypt Parquet file content in Hive tables, the information about which columns to encrypt with which keys can be stored in the Apache Hive Metastore, and is automatically applied by Spark whenever data is saved in the table files.

To configure Parquet encryption on Hive tables:

1. Create a table using the Hive Beeline CLI and configure which Parquet file content columns to encrypt with which keys by using  SERDEPROPERTIES. The following sample code snippet sets the encryption keys to use to encrypt the "credit_card" column (and the footer):

    ```
    CREATE TABLE my_table(name STRING, credit_card STRING)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
    WITH SERDEPROPERTIES (
        'parquet.encryption.column.keys'='c4a21521-2a78-4968-a7c2-57c481f58d5c: credit_card',
        'parquet.encryption.footer.key'='d1ae3fc2-6b7d-4a03-abb6-644e02933734')
    STORED AS parquet
    ```

    If you don't use the Hive Beeline CLI, you can use Spark SQL to setup parquet encryption on a Hive table that you create and pass the table-specific parquet encryption parameters via the OPTIONS keyword. For example:

    ```
    spark.sql("""
    CREATE TABLE my_table(name STRING, credit_card STRING)
    USING HIVE
    OPTIONS(
      parquet.encryption.column.keys 'c4a21521-2a78-4968-a7c2-57c481f58d5c: credit_card',
      parquet.encryption.footer.key 'd1ae3fc2-6b7d-4a03-abb6-644e02933734',
      fileFormat 'parquet')
      """)
    ```

    You can use `SERDEPROPERTIES` or `OPTIONS` to configure KMS properties too:
    ```
    CREATE TABLE my_table(name STRING, credit_card STRING)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
    WITH SERDEPROPERTIES (
      'parquet.encryption.kms.instance.url'='https://<region>.kms.cloud.ibm.com',
      'parquet.encryption.kms.instance.id'='<KeyProtect instance ID>',
      'parquet.encryption.column.keys'='c4a21521-2a78-4968-a7c2-57c481f58d5c: credit_card',
      'parquet.encryption.footer.key'='d1ae3fc2-6b7d-4a03-abb6-644e02933734')
    STORED AS parquet;
    ```
1. After you have created the table, add data to it by using the `DataFrameWriter.insertInto` operator.

    For example, you could load data from a CSV file, and add it to your table:
    ```
    df = spark.read.format("csv").load("/path/to/csv")
    df.write.insertInto("my_table")
    ```

## Usage examples
{: #usage-examples-parquet-encryption-hive}

The following examples show you how you can use Parquet encryption on data saved in Hive tables.

- Python example: Load data from a CSV file to a Hive table where the content is saved as encrypted Parquet file content with keys managed by IBM KeyProtect:

    ```python
    ACCESS_TOKEN = "A valid IAM access token"
    KEY_PROTECT_INSTANCE_ID = "My KP instance ID"
    K1 = "d1ae3fc2-6b7d-4a03-abb6-644e02933734"
    K2 = "c4a21521-2a78-4968-a7c2-57c481f58d5c"

    # setup key protect on the hadoop configuration
    hc = spark.sparkContext._jsc.hadoopConfiguration()
    hc.set("parquet.encryption.kms.instance.url", "https://<region>.kms.cloud.ibm.com")
    hc.set("parquet.encryption.kms.instance.id", KEY_PROTECT_INSTANCE_ID)
    hc.set("parquet.encryption.key.access.token", ACCESS_TOKEN)

    # load data from CSV
    csvDF = spark.read.format("csv").load("squares.csv")

    # create Hive table
    spark.sql("""
    CREATE TABLE squares_table(int_column int, square_int_column int)
    USING HIVE
    OPTIONS(
      parquet.encryption.column.keys '%s: square_int_column',
      parquet.encryption.footer.key '%s',
      fileFormat 'parquet')
    """ % (K1, K2))

    # write csvDF to hive table
    csvDF.write.insertInto("squares_table")

    # read from table and print
    spark.sql("SELECT * FROM squares_table").show()
    ```
- Python example: Load data from a CSV file to a Hive table where the content is saved as encrypted Parquet file content with keys managed by application:

    ```python
    # setup explicit keys on the hadoop configuration
    hc = sc._jsc.hadoopConfiguration()
    hc.set("parquet.encryption.key.list", "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")

    # load data from CSV
    csvDF = spark.read.format("csv").load("squares.csv")

    # create Hive table
    spark.sql("""
    CREATE TABLE squares_table(int_column int, square_int_column int)
    USING HIVE
    OPTIONS(
      parquet.encryption.column.keys 'key1: square_int_column',
      parquet.encryption.footer.key 'key2',
      fileFormat 'parquet')
    """)

    # write csvDF to hive table
    csvDF.write.insertInto("squares_table")

    # read from table and print
    spark.sql("SELECT * FROM squares_table").show()
    ```
- Python example: Read from a Hive table where the content is saved as encrypted Parquet file content with keys managed by IBM KeyProtect:

    ```python
    ACCESS_TOKEN = "A valid IAM access token"
    # setup access token for key protect on the Hadoop
    configurationspark.sparkContext._jsc.hadoopConfiguration().set("parquet.encryption.key.access.token", ACCESS_TOKEN)  
    # read from table and print
    spark.sql("SELECT * FROM squares_table").show()
    ```
