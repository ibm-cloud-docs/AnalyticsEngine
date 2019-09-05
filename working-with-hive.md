---

copyright:
  years: 2017, 2019

lastupdated: "2019-06-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Working with Hive and Hive LLAP
{: #working-with-hive}

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL.

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an {{site.data.keyword.iae_full_notm}} service, Hive commands can be executed through the Beeline client and by default, Hive uses Tez as its execution engine.

In the `AE 1.2 Hive LLAP` software package, LLAP (Live Long and Process) is enabled on the Hive server. LLAP enables performing sub-second SQL analytics on Hadoop by intelligently caching data in memory with persistent servers that instantly process the SQL queries. LLAP is an evolution of the Hive architecture and supports HiveQL. This means that you should not have to make any changes to your Hive queries.

The benefits of using LLAP include:
- LLAP enables sub-second querying in Hive by keeping all data and servers running in-memory all the time, while retaining the ability to scale elastically within a YARN cluster.
- LLAP is good for cloud use-cases because it caches data in memory and keeps it compressed, overcoming long cloud storage access times and stretching the amount of data you can fit in RAM.

Note that Hive is not available in the `AE 1.1 Spark` package. However, Hive is available in all AE 1.2 software packages. In the `AE 1.2 Hive LLAP` package, LLAP (Live Long and Process) is enabled by default.

## Prerequisites
{: #hive-prereqs}

To work with Hive (with and without LLAP), you need your cluster user credentials and the SSH and Hive JDBC endpoint details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}} service instance.

When fetching the Hive (without LLAP) endpoint, you need the details in the `hive_jdbc` attribute in the service credentials. For the Hive LLAP endpoint, you need the details in the `hive_interactive_jdbc` attribute.

Note that the Hive LLAP endpoint is available only in an {{site.data.keyword.iae_full_notm}} service instance created by using the  `AE 1.2 Hive LLAP` software package.


## Connecting to the Hive server

Connect to the Hive server by using with Beeline client. The command varies depending on if you are connecting to the Hive cluster with or without LLAP:

- Without LLAP enabled, issue:

 ```
 ssh clsadmin@chs-xxxxx-mn003.<changeme>.ae.appdomain.cloud
 beeline -u 'jdbc:hive2://chs-xxxxx-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p **********
 ```
- With LLAP, issue:

 ```
 ssh clsadmin@chs-xxxxx-mn003.<changeme>.ae.appdomain.cloud
 beeline -u 'jdbc:hive2://chs-xxxxx-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive-interactive' -n clsadmin -p **********
 ```

 where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).

The following examples show useful HiveQL statements.

- Example of the CREATE TABLE statement:

	`CREATE TABLE docs (line STRING);`

- Example of the LOAD statement:

	`LOAD DATA INPATH 'path_to_hdfs_data.txt' OVERWRITE INTO TABLE docs;`

- Example of the SELECT statement:

	`SELECT * from doc;`

## Accessing data in {{site.data.keyword.cos_full_notm}} from Hive  

Use the following example statement to access data in {{site.data.keyword.cos_full_notm}} from Hive:

```
CREATE EXTERNAL TABLE myhivetable( no INT, name STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION 'cos://<bucketname>.<servicename>/dir1';
```

`<bucketname>` is the {{site.data.keyword.cos_short}} bucket. The value for `<servicename>` can be any literal such as `iamservice` or `myprodservice`.


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
jdbc:mysql://<changeme>:<mySQLPortNumber>/compose
```

 where `<changeme>` is the endpoint to a database connection, for example to an instance of Compose in Dallas and `<mySQLPortNumber>` is your port number.
 For example:

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

 You can use this [sample script](https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/associate-external-metastore.sh){: external} to configure the Hive metastore.

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
4. Create an external table in Parguet format in {{site.data.keyword.cos_full_notm}}. Your cluster needs to be configured to use {{site.data.keyword.cos_short}}. See [Configuring clusters to work with {{site.data.keyword.cos_short}}](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos).
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

1. Create another external table in {{site.data.keyword.cos_short}} and view the values stored in Parquet format in the {{site.data.keyword.cos_short}} directory:

 ```
CREATE EXTERNAL TABLE parquet_test2 (x INT, y STRING) STORED AS PARQUET LOCATION 'cos://mybucket.myprodservice/dir2';
insert into parquet_test2 values (1,'Alex');
select * from parquet_test2;
```

1. Load data from a Parquet file stored in {{site.data.keyword.cos_short}} to an external Parguet table. `users-parquet` is a Parquet file stored in the {{site.data.keyword.cos_short}} bucket.
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

1. Load data from a Parquet file stored in HDFS into an external Parquet table. The `users.parquet` file is stored in the HDFS path `/user/hive`.
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
| NULL                 | Ben                    |
```


## ORC file format in Hive
{: #orc-format}

The Optimized Row Columnar (ORC) file format provides a highly efficient way to store Hive data. It is designed to overcome the limitations of other Hive file formats. Using ORC files improves performance when Hive is reading, writing, and processing data.

### Creating Hive tables in ORC format

To create Hive tables in ORC format:

 1. SSH to the cluster.
 2. Launch Beeline:

 ```
 beeline -u 'jdbc:hive2://XXXX-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p <yourClusterPassword>
 ```
 3. Create an external table in ORC format in {{site.data.keyword.cos_full_notm}}. To be able to do this, your cluster must have been [configured to work with {{site.data.keyword.cos_short}}](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos).

 ```
 CREATE EXTERNAL TABLE orc_table(line STRING) STORED AS ORC LOCATION 'cos://mybucket.myprodservice/ORC';
 ```
 4. Load data from an ORC file stored in {{site.data.keyword.cos_short}} into an external parquet table:

 ```
 LOAD DATA INPATH 'cos://mybucket.myprodservice/orc-file-11-format.orc' OVERWRITE INTO TABLE orc_table;
select * from orc_table;
```
`orc-file-11-format.orc` is an ORC file stored in the {{site.data.keyword.cos_short}} bucket.

## LLAP configuration on the cluster
{: #llap-config}

All the compute nodes on a Hive LLAP cluster are dedicated for LLAP related daemons. This means that there is no compute power left for  these nodes to run other workloads. Each compute node runs an LLAP daemon and some Tez query coordinators (based on the hardware type of the node) in Yarn containers.

The following table shows the LLAP configuration for one node for each of the supported hardware types.

| Per node configuration | `Default` node size | `Memory intensive` node size |
|-----------------|--------------|---------------------|
| Executor size | 3584 MB  | 4096 MB  |
| Number of executors | 2  | 24  |
| JVM overhead | 360 MB  | 6144 MB  |
| LLAP heap size | 7168 MB  | 98304 MB |
| Cache | 3736 MB  | 8192 MB  |
| LLAP daemon size | 11264 MB   | 112640 MB  |
| Tez coordinator size | 1024 MB  | 1024 MB  |
| Number of Tez coordinators| 1  |  4  |


## Code samples

Here is a Python code sample that shows accessing data in a Hive table on your cluster:

```python
import jaydebeapi;
conn = jaydebeapi.connect("org.apache.hive.jdbc.HiveDriver","jdbc:hive2://chs-mmm-007-mn001.us-south.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive",["clsadmin", "topsecret"],"/home/wce/clsadmin/hive-jdbc-uber-2.6.5.0-292.jar")
curs = conn.cursor();
curs.execute("select * from employees");
print(curs.fetchall())
print(curs.description)
curs.close()
```


## Learn more
{: #learn-more-hive}

- [Hive and its features](https://hortonworks.com/apache/hive/){: external}
- [Hive LLAP deep dive](https://community.hortonworks.com/articles/215868/hive-llap-deep-dive.html){: external}
- [Sample JDBC program that shows you how to use the Hive endpoints](https://github.com/IBM-Cloud/IBM-Analytics-Engine/tree/master/jdbcsamples/TestHive){: external}
- [Connecting SQuirrel with JDBC to Hive on IBM Analytics Engine](https://medium.com/@rakhi.sa/ibm-analytics-engine-how-to-connect-squirrel-with-jdbc-to-hive-on-ibm-analytics-engine-a23866961a63){: external}
