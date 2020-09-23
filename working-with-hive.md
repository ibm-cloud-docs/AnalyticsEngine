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

# Working with Hive and Hive LLAP
{: #working-with-hive}

The Apache Hive data warehousing software facilitates reading, writing, and managing large data sets that reside in distributed storage by using the SQL-like query language called HiveQL.

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an {{site.data.keyword.iae_full_notm}} service, Hive commands can be executed through the Beeline client and by default, Hive uses Tez as its execution engine.

In the `AE 1.2 Hive LLAP` software package, LLAP (Live Long and Process) is enabled on the Hive server by default. LLAP enables performing sub-second SQL analytics on Hadoop by intelligently caching data in memory with persistent servers that instantly process the SQL queries. LLAP is an evolution of the Hive architecture and supports HiveQL. This means that you should not have to make any changes to your Hive queries.

The benefits of using LLAP include:
- LLAP enables sub-second querying in Hive by keeping all data and servers running in-memory all the time, while retaining the ability to scale elastically within a YARN cluster.
- LLAP is good for cloud use-cases because it caches data in memory and keeps it compressed, overcoming long cloud storage access times and stretching the amount of data you can fit in RAM.

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

## Externalizing the Hive metastore to Databases for PostgreSQL
{: #externalizing-hive-metastore}

The Hive metastore is where the schemas of the Hive tables are stored. By default, it is in an embedded MySQL instance within the cluster. You should choose to externalize the metastore to an external database instance outside of the cluster so that you can tear down your cluster without losing any metadata. This, in combination with storing your data in {{site.data.keyword.cos_full_notm}}, helps persisting data across clusters. [Externalizing metadata](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-best-practices#separate-compute-from-storage) is a best practice when creating a cluster.

### Accessing Databases for PostgreSQL

IBM Cloud Databases for PostgreSQL is a database service that you should use to externalize the metadata of a cluster. PostgreSQL is an open source object-relational, enterprise-ready database.

After you have created an instance, you will need to note the administrative username, password, database name, hostname, port and the certificate details.

**Important**: Make sure you use the **private endpoint** to the PostgreSQL instance in the connection URL. Using the private endpoints increases performance and is more cost effective.

The PostgreSQL parameters to set in the Hive metastore include:

- **DB_USER_NAME**: The database user name to connect to the PostgreSQL instance you created, using the format: `ibm_cloud_<guid>`
- **DB_PWD**: The database user password with which to connect to the instance
- **DB_NAME**: The database name, typically 'ibmclouddb'
- **DB_CXN_URL**: The complete URL of database connection using the format:
```
jdbc:postgresql://<hostname>:<port>/<dbname>?sslmode=verify-ca&sslrootcert=<path-to-cert>
```
For example:
```
jdbc:postgresql://6b190ee0-44ed-4d84-959a-5b424490ccc6.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:31977/ibmclouddb?sslmode=verify-ca&sslrootcert=/home/wce/clsadmin/postgres.cert
```

### Copying the PostgreSQL certificate

To enable connecting to IBM Cloud Databases for PostgreSQL, you need to copy the self-signed certificate to the {{site.data.keyword.iae_full_notm}} cluster to a specific location. You can get the certificate details from the PostGreSQL instance details. The certificate is a base64 encoded string.

Use the following steps to copy the instance certificate to the {{site.data.keyword.iae_full_notm}} cluster:

1. Copy the certificate information in the Base64 field of the PostGreSQL connection information.
1. Decode the Base64 string to text and save it to a file. You can use the file name that is provided or use your own name.

 Alternatively, you can decode the certificate of your connection by using the following CLI plug-in command:
```
ibmcloud cdb deployment-cacert "your-service-name"
```
Copy and save the command's output to a file. See [CLI plug-in support for the self-signed certificate](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-external-app#cli-plug-in-support-for-the-self-signed-certificate).
1. Copy the file to `/home/wce/clsadmin` on the `mn002` node. To get to the `mn002` node, first `scp` the file to the `mn003` node from your local machine and then from there `scp` the file to the `mn002` node.

  Alternatively, you can copy directly to the `mn002` node by using the `mn003` as a jump host using the following command:
  ```
  scp -J clsadmin@chs-xxxxx-mn003.<region>.ae.appdomain.cloud <post-gres.cert> clsadmin@chs-xxxxx-mn002.<region>.ae.appdomain.cloud:/home/common/wce/clsadmin/
  ```
1. Enter `chmod -R 755 /home/wce/clsadmin` to grant access permissions to the certificate file in the folder.

### Configuring a cluster to work with PostgreSQL

Adhoc PostgreSQL customization scripts can be used to configure the cluster to work with PostgreSQL. See [Running an adhoc  customization script for configuring Hive with a Postgres external metastore](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-cust-examples#postgres-metastore).

Alternatively, you can use the Ambari user interface after you have created a cluster, to add the PostgreSQL connection values to the `hive-site.xml` file.

To configure a cluster to work with your PostgreSQL instance:

1. From the Ambari dashboard,  open the advanced configuration for Hive by clicking **Hive > Configs > Database Tab > Hive Database > Existing PostgreSQL**.
1. Add the PostgreSQL values for your instance to the following parameters: `Database Name`, `Database Username`, `Database Password`, `Database URL`.
1. Save your changes. You must restart affected services as indicated in the Ambari user interface so that the changes take effect. This could take approximately three minutes.

**Note**:
- You don't have to install the PostgreSQL JDBC driver nor run any Ambari setup steps as this has been preconfigured on the cluster.  
- You might not be able to click **Test Connection**  because of a known issue in the Ambari user interface. Also note that you can ignore the warnings and confirm to all prompts in the UI while saving.


## Parquet file format in Hive
{: #parquet}

Parquet is an open source file format for Hadoop. Parquet stores nested data structures in a flat columnar format. Compared to the traditional approach where data is stored in rows, Parquet is more efficient in terms of storage and performance.

### Creating Hive tables in Parquet format

To create Hive tables in Parquet format:

1. SSH to the cluster.

2. Launch Beeline:
```
beeline -u 'jdbc:hive2://XXXX-mn001.<changeme>.ae.appdomain.cloud:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p <yourClusterPassword>
```

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
| NULL                 | Ben                    |
```
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
{: #code-samples-hive}

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
