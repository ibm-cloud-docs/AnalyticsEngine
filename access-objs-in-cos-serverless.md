---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Accessing objects in {{site.data.keyword.cos_short}}
{: #access-objs-in-cos-serverless}

After you have configured {{site.data.keyword.iae_full_notm}} to work with {{site.data.keyword.cos_short}}, you can start accessing objects in {{site.data.keyword.cos_short}} from Spark.

When running Spark jobs, the system is configured to use IBM’s open source Stocator libraries that offer better performance and optimization for large object reads and writes as compared to the default AWS connectors. See  [Stocator - Storage Connector for Apache Spark](https://github.com/CODAIT/stocator){: external}.

To access data objects in an {{site.data.keyword.cos_short}} bucket, use the following URI:
```
cos://<bucket_name>.<servicename>/<object_name>
```
where `<bucket>` is the {{site.data.keyword.cos_short}} bucket name, for example `b1` and <service> identifies configuration group entry, for example `myObjectStorageInstance`.

For example:
```
cos://b1.myObjcetStorageInstance/detail.txt
```
