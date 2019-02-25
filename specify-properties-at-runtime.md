---

copyright:
  years: 2017, 2019
lastupdated: "2018-09-25"

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

For example, you can specify the properties at runtime in your Python, Scala, or R code when executing jobs. The following snippet shows an example for Spark:

```
prefix="fs.cos.myprodservice"

hconf=sc._jsc.hadoopConfiguration()
hconf.set(prefix + ".iam.endpoint", "https://iam.bluemix.net/identity/token")
hconf.set(prefix + ".endpoint", "s3-api.us-geo.objectstorage.service.networklayer.com")
hconf.set(prefix + ".iam.api.key", "he0Zzjasdfasdfasdfasdfasdfasdfj2OV")
hconf.set(prefix + ".iam.service.id", "ServiceId-asdf-asdf-asdf-asdf-asdf")

t1=sc.textFile("cos://mybucket.myprodservice/tata.data")
t1.count()
```
{: codeblock}
