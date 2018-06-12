---

copyright:
  years: 2017,2018
lastupdated: "2018-06-11"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Hive

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL.

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an {{site.data.keyword.iae_full_notm}} service, Hive commands can be executed through the Beeline client and by default, the Hive uses Tez as its execution engine. Note that Hive is not available in the {{site.data.keyword.iae_short}} Spark package.

## Prerequisites
To work with Hive, you need your cluster user credentials and the ssh and hive_jdbc end point details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}} service instance.

## Connecting to the Hive server

Connect to the Hive server by using with beeline client.

Issue the following SSH command to the cluster:

```
ssh clsadmin@chs-xxxxx-mn003.bi.services.<changeme>.bluemix.net
beeline -u 'jdbc:hive2://chs-xxxxx-mn001.bi.services.<changeme>.bluemix.net:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p **********
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

The following examples show useful HiveQL statements.

- Example of the CREATE TABLE statement:

	`CREATE TABLE docs (line STRING);`

- Example of the LOAD statement:

	`LOAD DATA INPATH 'path_to_hdfs_data.txt' OVERWRITE INTO TABLE docs;`

- Example of the SELECT statement:

	`SELECT * from doc;`

## Accessing data in IBM CLoud Object Storage S3 from Hive  

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

* By using the Ambari user interface after the cluster has been created
* By configuring the cluster as part of the cluster customization script
 <br>

##### Using the Ambari user interface after the cluster was created

To configure the cluster:

1. Add the properties and values to the hive-site.xml file on your cluster instance by opening the Ambari console.
2. Open the advanced configuration for HIVE:

  **Ambari dashboard > Hive > Config > Advanced Tab > Hive Metastore > Existing MySQL / MariaDB Database**

3. Make the appropriate changes for the following parameters: Database Name, Database Username, Database Password, Database URL.
4. Save your changes.

**Important**: You will need to restart affected services as indicated in the Ambari user interface so that the changes take effect. This could take approximately three minutes.

**Note**: You might not be able to click **Test Connection** because of a known issue in the Ambari user interface.

##### Configuring the cluster as part of the cluster customization script

You can use a customization script when the cluster is created. This script includes the properties that need to be configured in the Hive site and uses the Ambari configs.py file to make the required changes.

Use this [sample script](https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/associate-external-metastore.sh) to configure the Hive metastore.

## Learn more

- [Hive and its features](https://hortonworks.com/apache/hive/).
- [Sample JDBC program that shows you how to use the Hive endpoints](https://github.com/IBM-Cloud/IBM-Analytics-Engine/tree/master/jdbcsamples/TestHive)
