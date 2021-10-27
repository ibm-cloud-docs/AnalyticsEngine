---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-19"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Spark SQL to query data
{: #working-with-sql}

Spark SQL is a Spark module for processing structured data and serves as a distributed SQL engine, allowing it to leverage YARN to manage memory and CPUs in the cluster. With Spark SQL, you can query data in Hive databases and other data sets. Spark SQL supports Hive data formats, user-defined functions (UDFs), and the Hive metastore.

One use of Spark SQL is to execute SQL queries. Spark SQL can also be used to read data from an existing Hive installation. When running SQL from within another programming language, the results are returned in a Spark DataFrame. You can also interact with the SQL interface using the command-line interface or over JDBC/ODBC. All of the following code samples use sample data included in the Spark distribution and can be run in the Spark shell, the PySpark shell, or the SparkR shell.

## Prerequisites
{: #spark-sql-prereqs}

To work with Spark SQL, you need your cluster user credentials and the SSH and Spark sql_jdbc endpoint details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}}  service instance.

## Connecting to the Spark SQL server by using Beeline

You can connect to the Spark SQL server by using the Beeline client.

1. Issue the following SSH command to the cluster and launch Beeline by providing the Spark SQL endpoint:

    ```
    ssh clsadmin@chs-xxxxx-mn003.<changeme>.ae.appdomain.cloud
    /usr/hdp/current/spark2-client/bin/beeline -u 'jdbc:hive2://chs-xxxxx-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/spark' -n clsadmin -p **********
    ```
    `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).

    After its successfully connected, the following message is displayed which shows it is connected to Spark SQL.

    ```
    Connecting to jdbc:hive2://chs-pqj-477-mn001.eu-de.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/spark
    Connected to: Spark SQL (version 2.3.0.2.6.5.0-292)
    Driver: Hive JDBC (version 1.2.1000.2.6.5.0-292)
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    Beeline version 1.2.1000.2.6.5.0-292 by Apache Hive
    0: jdbc:hive2://chs-pqj-477-mn001.eu-de.ae.ap>
    ```
2. Now you can create an external table in IBM Cloud Object Storage and load data to this table, for example:

    ```
    CREATE EXTERNAL TABLE PEOPLE(age LONG, name STRING) LOCATION 'cos://kpbucket.myprodservice/people';
    LOAD DATA LOCAL INPATH '/usr/hdp/current/spark2-client/examples/src/main/resources/people.json' INTO TABLE PEOPLE;
    select name, age from PEOPLE;
    ```

## Running Spark SQL with Scala

To run Spark SQL with Scala:

1. Launch the Spark shell:

    ```
    spark shell \
     --master  yarn \
     --deploy-mode client
    ```
2. Then run the following code which reads sample data from a CSV file, loads it to a DataFrame, queries for people in the people table and then displays the name, age and job of the persons found:

    ```
    val sqlContext = new org.apache.spark.sql.SQLContext(sc)
    val df = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("inferSchema", "true").load("file:///usr/hdp/current/spark2-client/examples/src/main/resources/people.csv")
    df.show()
    df.printSchema()
    // Register the DataFrame as a SQL temporary view
    df.createOrReplaceTempView ("People")
    val sqlDF =sqlContext.sql("select * from People")
    sqlDF.show()
    +------------------+
    |      name;age;job|
    +------------------+
    |Jorge;30;Developer|
    |  Bob;32;Developer|
    +------------------+
    ```

## Running Spark SQL with Python 3

To run Spark SQL with Python 3:

1. Launch the PySpark shell:

    ```
    PYSPARK_PYTHON=/home/common/conda/miniconda3.7/bin/python pyspark \
    --master  yarn \
    --deploy-mode client
    ```
2. Then run the following code which reads sample data from a JSON file to a Parquet file, loads it to a DataFrame, queries for all teenagers between the ages of 13 and 19 and then displays the found results:

    ```
    peopleDF = spark.read.json("file:///usr/hdp/current/spark2-client/examples/src/main/resources/people.json")
    # DataFrames can be saved as Parquet files, maintaining the schema information.
    peopleDF.write.parquet("people.parquet")
    # Read the created Parquet file.
    # Parquet files are self-describing so the schema is preserved.
    # The result of loading a parquet file is also a DataFrame.
    parquetFile = spark.read.parquet("people.parquet")
    # Parquet files can also be used to create a temporary view and then used in SQL statements.
    parquetFile.createOrReplaceTempView("parquetFile")
    teenagers = spark.sql("SELECT name FROM parquetFile WHERE age >= 13 AND age <= 19")
    teenagers.show()
    +------+                                                                        
    |  name|
    +------+
    |Justin|
    ```

## Running Spark SQL with R

To run Spark SQL with R:

1. Launch the SparkR shell:

    ```
    spark shell \
    --master  yarn \
    --deploy-mode client
    ```
1. Then run the following code which creates a Hive table called src if it does not already exist, loads data into this table,and then queries the table for particular key-value pairs:

    ```
    // enableHiveSupport defaults to TRUE
    sparkR.session(enableHiveSupport = TRUE)
    sql("CREATE TABLE IF NOT EXISTS src (key INT, value STRING) USING hive")
    sql("LOAD DATA LOCAL INPATH 'file:///usr/hdp/current/spark2-client/examples/src/main/resources/kv1.txt' INTO TABLE src")
    // Queries can be expressed in HiveQL.
    results <- collect(sql("FROM src SELECT key, value"))
    head(results)
    +------+
    key   value
    1 238 val_238
    2  86 val_86
    3 311 val_311
    4  27 val_27
    5 165 val_165
    6 409 val_409
    ```

## Running the Spark SQL CLI

The Spark SQL CLI is a convenient tool to run the Hive metastore service in local mode and execute queries input from the command line.

1. Start the Spark SQL CLI by running the following commands in the Spark directory `/usr/hdp/current/spark2-client`:

    ```
    ./bin/spark-sql
    show tables;
    18/10/04 05:56:41 INFO CodeGenerator: Code generated in 382.468966 ms
    default	people	false
    default	src	false
    Time taken: 3.717 seconds, Fetched 2 row(s)
    18/10/04 05:56:41 INFO SparkSQLCLIDriver: Time taken: 3.717 seconds, Fetched 2 row(s)
    ```
1. Now you can create tables, load data to the tables, and then query the contents in those tables as shown in previous code samples.
