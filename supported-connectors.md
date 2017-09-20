---

copyright:
  years: 2017
lastupdated: "2017-09-07"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

#  Spark connectors
The following connectors are currently provided by default on all IBM Analytics Engine clusters:

 * Db2 and dashdDB
  * DB2 JDBC
  * DB2 ODBC
  * dashDB Idax Data Source(com.ibm.idax.spark.idaxsource)
 * Cloudant(#Cloudant)
 * Stocator swift
 * Stocator COS
 * OpenStack COS

## Db2 and dashDB Connector

dashDB (now also known as Db2 on Cloud and Db2 Warehouse on Cloud) is a fully managed cloud data warehouse, purpose-built for analytics. It offers massively parallel processing (MPP) scale, and compatibility with a wide range of business intelligence (BI) tools.

### Sample python code to access dashDB using JDBC
```
credentials_1 = {
'jdbcurl':'jdbc:db2://YOUR_DATABASE_HOSTNAME:50000/YOUR_DATABASE_NAME',
 'username':'YOUR_DATABASE_USERNAME',
 'password':'YOUR_DATABASE_PASSWORD’
}
transportation = sqlContext.read.jdbc(credentials_1["jdbcurl"],"YOUR_DATABASE_TABLE",properties = {"user" : credentials_1["username"], "password" : credentials_1["password"],"driver" : "com.ibm.db2.jcc.DB2Driver"})
transportation.show()
```
{: codeblock}

### Sample python code to access dashDB using ODBC
```
from ibmdbpy import IdaDataBase, IdaDataFrame
idadb_1 = IdaDataBase(dsn='DASHDB;Database=YOUR_DB_NAME;Hostname=YOUR_DATABASE_HOSTNAME;Port=YOUR_DATABASE_PORT;PROTOCOL=TCPIP;UID= YOUR_DATABASE_USERNAME;PWD= YOUR_DATABASE_PASSWORD')
ida_df_1 = IdaDataFrame(idadb_1, YOUR_DATABASE_TABLE_NAME',indexer="YOUR_TABLE_COLUMN")
ida_df_1.head()
```
{: codeblock}

### Sample python code to access dashDB using the idax connector
```
credentials_1 = {
'jdbcurl':'jdbc:db2://YOUR_DATABASE_HOSTNAME:50000/YOUR_DATABASE_NAME',
 'username':'YOUR_DATABASE_USERNAME',
 'password':'YOUR_DATABASE_PASSWORD’
}
    inputData = spark.read
     .format("com.ibm.idax.spark.idaxsource")
      .options(dbtable="YOUR_TABLE")
      .options(**credentials_1)
     .load()

print(inputData.show())
```
{: codeblock}

## Cloudant
```
db_name = "YOUR_CLOUDANT_DBNAME"
data_df = sqlContext.read.format("com.cloudant.spark").option("cloudant.host","YOUR_CLOUDANT_USERNAME.cloudant.com").option("cloudant.username"," YOUR_CLOUDANT_USERNAME ").option("cloudant.password"," YOUR_CLOUDANT_PASSWORD
```
{: codeblock}

## Sample code to access data from Swift storage

Update the CREDENTIALS variable with your Swift object storage details
```
credentials = {
 'auth_url':'’,
 'project': ',
 'project_id':'',
 'region':'',
 'user_id':'',
 'domain_id':'',
 'domain_name':'',
 'username':',
 'password': '’,
 'filename':'store_sales_1g.dat',
 'container':'',
 'tenantid':’’,
 'name':''
}
swift2d_driver="com.ibm.stocator.fs.objectstorefilesystem"

prefix = "fs.swift2d.service." + credentials['name']
hconf = sc._jsc.hadoopconfiguration()
hconf.set("fs.swift2d.impl", swift2d_driver)
hconf.set(prefix + ".auth.url", credentials['auth_url']+'/v3/auth/tokens')
hconf.set(prefix + ".auth.endpoint.prefix", "endpoints")
hconf.set(prefix + ".tenant", credentials['project_id'])
hconf.set(prefix + ".username", credentials['user_id'])
hconf.set(prefix + ".password", credentials['password'])
hconf.setint(prefix + ".http.port", 8080)
hconf.set(prefix + ".region", credentials['region'])
hconf.setboolean(prefix + ".public", false)

url = "swift2d://<container>.keystone/store_sales_1g.dat"
data = sc.textfile(url)
print(data.count())
}        
```
{: codeblock}
