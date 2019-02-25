---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-14"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Hive
{: #working-with-hive}

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL.

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an {{site.data.keyword.iae_full_notm}} service, Hive commands can be executed through the Beeline client and by default, the Hive uses Tez as its execution engine. Note that Hive is not available in the {{site.data.keyword.iae_short}} Spark package.

- [Prerequisites](#prerequisites)
- [Connecting to the Hive server](#connecting-to-the-hive-server)
- [Accessing data in IBM Cloud Object Storage S3 from Hive](#accessing-data-in-ibm-cloud-object-storage-s3-from-hive)
- [Changing the Hive execution engine](#changing-the-hive-execution-engine)
- [Externalizing the Hive metastore to IBM Compose for MySQL](#externalizing-the-hive-metastore-to-ibm-compose-for-mysql)
- [Parquet file format in Hive](#parquet)
- [ORC file format in Hive](#orc-format)
- [Learn more](#learn-more)


## Prerequisites
To work with Hive, you need your cluster user credentials and the ssh and hive_jdbc end point details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}} service instance.

## Connecting to the Hive server

Connect to the Hive server by using with Beeline client.

Issue the following SSH command to the cluster:

```
ssh clsadmin@chs-xxxxx-mn003.<changeme>.ae.appdomain.cloud
beeline -u 'jdbc:hive2://chs-xxxxx-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p **********
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).

The following examples show useful HiveQL statements.

- Example of the CREATE TABLE statement:

	`CREATE TABLE docs (line STRING);`

- Example of the LOAD statement:

	`LOAD DATA INPATH 'path_to_hdfs_data.txt' OVERWRITE INTO TABLE docs;`

- Example of the SELECT statement:

	`SELECT * from doc;`

## Accessing data in IBM Cloud Object Storage S3 from Hive  

Use the following example statement to access data in IBM Cloud Object Storage (COS) from Hive:
```
CREATE EXTERNAL TABLE myhivetable( no INT, name STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION 'cos://<bucketname>.<servicename>/dir1'; ```

`<bucketname>` is the COS bucket. The value for `<servicename>` can be any literal such as `iamservice` or `myprodservice`.


## Changing the Hive execution engine

To change the Hive execution engine from Tez to MR, run the following command in the beeline client prompt:

`set hive.execution.engine=mr;`

## Externalizing the Hive metastore to IBM Compose for MySQL

The Hive metastore is where the schemas of the Hive tables are stored. By default, it is in a MySQL instance in the cluster. You could choose to externalize the metastore to an external MySQL instance outside of the cluster so that you can tear down your cluster without losing the metadata. This, in combination with data in the object store, can preserve the data across clusters.

### Compose for MySQL
Compose for MySQL is a service in {{site.data.keyword.Bluemix_notm}} that can be used to externalize the metadata of the cluster. You can choose between the Standard or Enterprise version depending on your requirement. Once you create the Compose for MySQL instance, you will need to note the administrative user, password, database name, and the JDBC URL.

The Compose for MySQL parameters to set in the Hive metastore include:

- **DB_USER_NAME**: The database user name to connect to the instance, which is typically *admin*.

- **DB_PWD**: The database user password to connect to the instance.

- **DB_NAME**: The database name, which is typically *compose*.

- **DB_CXN_URL**: The complete database connection URL.
```
jdbc:mysql://<changeme>:<mySQLPortNumber>/compose ```

 where `<changeme>` is the endpoint to a database connection, for example to an instance of Compose in Dallas and `<mySQLPortNumber>` is your port number.

 ```
 jdbc:mysql://bluemix-sandbox-dal-9-portal.6.dblayer.com:12121/compose ```

#### Configuring clusters to work with Compose for MySQL

There are two ways in which you can configure your cluster with the Compose for MySQL parameters:

-  By using the Ambari user interface after the cluster has been created
-  By configuring the cluster as part of the cluster customization script
 <br>

To configure the cluster using the Ambari user interface after the cluster was created:

1. Add the properties and values to the hive-site.xml file on your cluster instance by opening the Ambari console.
2. Open the advanced configuration for HIVE:

  **Ambari dashboard > Hive > Config > Advanced Tab > Hive Metastore > Existing MySQL / MariaDB Database**

3. Make the appropriate changes for the following parameters: Database Name, Database Username, Database Password, Database URL.
4. Save your changes.

 **Important**: You will need to restart affected services as indicated in the Ambari user interface so that the changes take effect. This could take approximately three minutes.

 **Note**: You might not be able to click **Test Connection** because of a known issue in the Ambari user interface.

To configuring the cluster as part of the cluster customization script:

1. Use a customization script after the cluster is created. This script includes the properties that need to be configured in the Hive site and uses the Ambari configs.py file to make the required changes.

 You can use this [sample script](https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/associate-external-metastore.sh) to configure the Hive metastore.

## Parquet file format in Hive
{: #parquet}

Parquet is an open source file format for Hadoop. Parquet stores nested data structures in a flat columnar format. Compared to the traditional approach where data is stored in rows, Parquet is more efficient in terms of storage and performance.

### Creating Hive tables in Parquet format

To create Hive tables in Parquet format:

1. SSH to the cluster.

2. Launch Beeline:
```
beeline -u 'jdbc:hive2://XXXX-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p <yourClusterPassword> ```

3. Create a Hive table in Parquet format:
```
CREATE TABLE parquet_test (
 id int,
 str string,
 mp MAP<STRING,STRING>,
 lst ARRAY<STRING>,
 strct STRUCT<A:STRING,B:STRING>)
PARTITIONED BY (part string)
STORED AS PARQUET;
```
4. Create an external table in Parguet format in IBM Cloud Object Storage. Your cluster needs to be configured to use Cloud Object Store. See [Configuring clusters to work with IBM COS S3 object stores](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos).
```
CREATE EXTERNAL TABLE parquet_test1 (
 id int,
 str string,
 mp MAP<STRING,STRING>,
 lst ARRAY<STRING>,
 strct STRUCT<A:STRING,B:STRING>)
PARTITIONED BY (part string)
STORED AS PARQUET LOCATION 'cos://mybucket.myprodservice/dir1';
```

1. Create another external table in IBM Cloud Object Storage and view the values stored in Parquet format in the Cloud Object Storage directory:

 ```
CREATE EXTERNAL TABLE parquet_test2 (x INT, y STRING) STORED AS PARQUET LOCATION 'cos://mybucket.myprodservice/dir2';
insert into parquet_test2 values (1,'Alex');
select * from parquet_test2;
```

1. Load data from a Parquet file stored in Cloud Object Storage to an external Parguet table. users-parquet is a Parquet file stored in the Cloud Object Storage bucket.
```
CREATE EXTERNAL TABLE extparquettable1 (id INT, name STRING) STORED AS PARQUET LOCATION 'cos://mybucket.myprodservice/dir3';
LOAD DATA INPATH 'cos://mybucket.myprodservice/dir6/users-parquet';
OVERWRITE INTO TABLE extparquettable1;
select * from extparquettable1;
```   
The result is the following:
```
| extparquettable1.id  | extparquettable1.name  |
|----------------------|------------------------|
| NULL                 | Alyssa                 |
| NULL                 | Ben                    |```

1. Load data from a Parquet file stored in HDFS into an external Parquet table. The users.parquet file is stored in the HDFS path `/user/hive`.
```
CREATE EXTERNAL TABLE extparquettable2 (id INT, name STRING) STORED AS PARQUET LOCATION 'cos://mybucket.myprodservice/dir1';
LOAD DATA INPATH 'users-parquet';
OVERWRITE INTO TABLE extparquettable2;
select * from extparquettable2;
```   
The result is the following:
```
| extparquettable2.id  | extparquettable2.name  |
|----------------------|------------------------|
| NULL                 | Alyssa                 |
| NULL                 | Ben                    |```


## ORC file format in Hive
{: #orc-format}

The Optimized Row Columnar (ORC) file format provides a highly efficient way to store Hive data. It is designed to overcome the limitations of other Hive file formats. Using ORC files improves performance when Hive is reading, writing, and processing data.

### Creating Hive tables in ORC format

To create Hive tables in ORC format:

 1. SSH to the cluster.
 2. Launch Beeline:
 ```
 beeline -u 'jdbc:hive2://XXXX-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p <yourClusterPassword>```
 3. Create an external table in ORC format in IBM Cloud Object Storage. To be able to do this, your cluster must have been [configured to work with Cloud Object Storage](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos).
 ```
 CREATE EXTERNAL TABLE orc_table(line STRING) STORED AS ORC LOCATION 'cos://mybucket.myprodservice/ORC'; ```
 4. Load data from an ORC file stored in Cloud Object Storage into an external parquet table:
 ```
 LOAD DATA INPATH 'cos://mybucket.myprodservice/orc-file-11-format.orc' OVERWRITE INTO TABLE orc_table;
select * from orc_table;
```
`orc-file-11-format.orc` is an ORC file stored in the Cloud Object Storage bucket.



## Learn more

- [Hive and its features](https://hortonworks.com/apache/hive/).
- [Sample JDBC program that shows you how to use the Hive endpoints](https://github.com/IBM-Cloud/IBM-Analytics-Engine/tree/master/jdbcsamples/TestHive)
- [Connecting SQuirrel with JDBC to Hive on IBM Analytics Engine](https://medium.com/@rakhi.sa/ibm-analytics-engine-how-to-connect-squirrel-with-jdbc-to-hive-on-ibm-analytics-engine-a23866961a63)
