---

copyright:
  years: 2017, 2021
lastupdated: "2021-01-07"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Specifying properties at runtime
{: #specify-props-runtime}

To enable an application to connect to {{site.data.keyword.cos_full_notm}}, you must update the {{site.data.keyword.iae_full_notm}} cluster configuration file (the core-site.xml file) to include credentials and other required connection values. One way of doing this is to configure the properties that need to be updated in the core-site.xml file at runtime.

The following snippets show useful commands that you can use with  runtime parameters:

- Sample HDFS list command with runtime parameters
  ```
  hadoop fs -Dfs.cos.cosinstance1.iam.api.key=lipsum199 -D fs.cos.cosinstance1.endpoint=s3.private.us.cloud-object-storage.appdomain.cloud -ls cos://b1.cosinstance1/alpha.data
  ```
  {: codeblock}

  You can repeat the same command for `cat`, `put`, `mkdir` and so on.
- Sample YARN job with runtime parameters:
  ```
  yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount -Dfs.cos.cosinstance1.iam.api.key=lipsum199 -Dfs.cos.cosinstance1.endpoint=s3.private.us.cloud-object-storage.appdomain.cloud  cos://b1.cosinstance1/input cos://b1.cosinstance1/wordcount/output
  ```
  {: codeblock}

- Sample Scala code that uses runtime parameters:
  ```scala
  val prefix="fs.cos.cosinstance1"
  sc.hadoopConfiguration.set(prefix + ".endpoint", "s3.private.us.cloud-object-storage.appdomain.cloud")
  sc.hadoopConfiguration.set(prefix + ".iam.api.key","lipsum999….")
  val data = Array(1, 2, 3, 4)
  val myData = sc.parallelize(data)
  myData.saveAsTextFile("cos://b1.cosinstance1/alpha.data")
  val t1=sc.textFile("cos://b1.cosinstance1/alpha.data")
  t1.count()
  ```
  {: codeblock}

- Sample Python code with runtime parameters:
  ```
  prefix="fs.cos.cosinstance1"
  hconf=sc._jsc.hadoopConfiguration()
  hconf.set(prefix + ".endpoint", "s3.private.us.cloud-object-storage.appdomain.cloud")
  hconf.set(prefix + ".iam.api.key", " lipsum999….")
  t1=sc.textFile("cos://b1.cosinstance1/alpha.data ")
  t1.count()
  ```
  {: codeblock}

- Sample Livy application that uses runtime parameters:
  ```
  curl -u "clsadmin:pwd" -H 'Content-Type: application/json' -H 'X-Requested-By: livy'
  -d '{
    "file": "/user/clsadmin/myapplications.py",
    "proxyUser": "clsadmin",
    "conf": {
      "spark.hadoop.fs.cos.cosinstance1.access.key": "a7634d…",
      "spark.hadoop.fs.cos.cosinstance1.secret.key": "5c09be…",
      "spark.hadoop.fs.cos.cosinstance1.endpoint": "s3.private.us-south.cloud-object-storage.appdomain.cloud"
      }
    }'
  https://chs-mmm-007-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches
  ```
  {: codeblock}
