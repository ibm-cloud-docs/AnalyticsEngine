---

copyright:
  years: 2017,2018
lastupdated: "2018-09-17"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Spark SQL to query data

Spark SQL is a module in Spark and serves as a distributed SQL engine, with which you can leverage YARN to manage memory and CPUs in your cluster. With Spark SQL, you can query existing Hive databases and other data sets.

**Prerequisites**

To work with Spark SQL, you need your cluster user credentials and the SSH and Spark sql_jdbc end point details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}}  service instance.

**Connecting to the Spark SQL server**

You can connect to the Spark SQL server by using the Beeline client.

Issue the following SSH command to connect to Spark SQL on the cluster:
```
ssh clsadmin@chs-xxxxx-mn003.bi.services.<changeme>.bluemix.net
beeline -u 'jdbc:hive2://chs-xxxxx-mn001.bi.services.<changeme>.bluemix.net:8443/;ssl=true;transportMode=http;httpPath=gateway/default/spark' -n clsadmin -p **********
```
`<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

After you have connected to Spark SQL on the cluster, the following message is displayed which shows that your connection was a success.

```
Connecting to jdbc:hive2://chs-qya-139-mn001.bi.services.us-south.bluemix.net:8443/;ssl=true;transportMode=http;httpPath=gateway/default/spark
Connected to: Spark SQL (version 2.3.0.2.6.5.0-292)
Driver: Hive JDBC (version 1.2.1000.2.6.5.0-292)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 1.2.1000.2.6.5.0-292 by Apache Hive
0: jdbc:hive2://<changeme>-mn003.bi.services>
```
