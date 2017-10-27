---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Hive 

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL. 

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an IBM Analytics Engine service, Hive commands can be executed through the Beeline client and by default, the Hive uses Tez as its execution engine. Note that Hive is not available in the Analytics Engine Spark package. 

## Prerequisites
To work with Hive, you need your cluster user credentials and the ssh and hive_jdbc end point details. You can get this information from the service credentials of your Analytics Engine service instance.

## Connecting to the Hive server

Connect to the Hive server by using with beeline client. 

Issue the following SSH command to the cluster:

```
ssh clsadmin@chs-xxxxx-mn003.bi.services.us-south.bluemix.net
beeline -u 'jdbc:hive2://chs-xxxxx-mn001.bi.services.us-south.bluemix.net:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p **********
```

The following examples show useful HiveQL statements.

- Example of the CREATE TABLE statement:

	`CREATE TABLE docs (line STRING);`

- Example of the LOAD statement:

	`LOAD DATA INPATH 'path_to_hdfs_data.txt' OVERWRITE INTO TABLE docs;`

- Example of the SELECT statement:

	`SELECT * from doc;`

- Example of a CREATE TABLE statement with data in COS object store:

	`CREATE EXTERNAL TABLE s3aTable( no INT, name STRING) 
	ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
	LOCATION 's3a://mybucket/myhivedir';` 


## Changing the Hive execution engine

To change the hive execution engine from Tez to MR, run the following command in the beeline client prompt:
 
`set hive.execution.engine=mr;`

For further information on Hive and its features, see [Apache Hive](https://hortonworks.com/apache/hive/).



