---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
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
hadoop fs -Dfs.cos.firstbucket.iam.api.key=lipsum199 -D fs.cos.firstbucket.endpoint=s3.private.us.cloud-object-storage.appdomain.cloud -ls cos://b1.firstbucket/alpha.data
```

 You can repeat the same command for `cat`, `put`, `mkdir` and so on.
- Sample YARN job with runtime parameters:
```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount -Dfs.cos.firstbucket.iam.api.key=lipsum199 -Dfs.cos.firstbucket.endpoint=s3.private.us.cloud-object-storage.appdomain.cloud  cos://b1.firstbucket/input cos://b1.firstbucket/wordcount/output
```
- Sample Scala code that uses runtime parameters:
```scala
val prefix="fs.cos.firstbucket"
sc.hadoopConfiguration.set(prefix + ".endpoint", "s3.private.us.cloud-object-storage.appdomain.cloud")
sc.hadoopConfiguration.set(prefix + ".iam.api.key","lipsum999….")
val data = Array(1, 2, 3, 4)
val myData = sc.parallelize(data)
myData.saveAsTextFile("cos://b1.firstbucket/alpha.data")
val t1=sc.textFile("cos://b1.firstbucket/alpha.data")
t1.count()
```
- Sample Python code with runtime parameters:
```
prefix="fs.cos.firstbucket"
hconf=sc._jsc.hadoopConfiguration()
hconf.set(prefix + ".endpoint", "s3.private.us.cloud-object-storage.appdomain.cloud")
hconf.set(prefix + ".iam.api.key", " lipsum999….")
t1=sc.textFile("cos://b1.firstbucket/alpha.data ")
t1.count()
```

<!-- For example, you can specify the properties at runtime in your Python, Scala, or R code when executing jobs. The following snippet shows an example for Spark:

```
prefix="fs.cos.myprodservice"

hconf=sc._jsc.hadoopConfiguration()
hconf.set(prefix + ".iam.endpoint", "https://iam.cloud.ibm.com/identity/token")
hconf.set(prefix + ".endpoint", "s3-api.us-geo.objectstorage.service.networklayer.com")
hconf.set(prefix + ".iam.api.key", "he0Zzjasdfasdfasdfasdfasdfasdfj2OV")
hconf.set(prefix + ".iam.service.id", "ServiceId-asdf-asdf-asdf-asdf-asdf")

t1=sc.textFile("cos://mybucket.myprodservice/tata.data")
t1.count()
```
{: codeblock} -->
