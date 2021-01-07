---

copyright:
  years: 2017, 2021
lastupdated: "2021-01-05"

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
{: #access-objs-in-cos}

After you have configured {{site.data.keyword.iae_full_notm}} to work with {{site.data.keyword.cos_short}}, you can start accessing objects in {{site.data.keyword.cos_short}} from Spark, Hive, or HDFS.

{{site.data.keyword.iae_full_notm}} uses HDP’s default AWS open source object storage connectors to access data from {{site.data.keyword.cos_short}} when running HDFS, Hive, or Mapreduce jobs.

However, when running Spark jobs, the system is preconfigured to use IBM’s open source Stocator libraries that offer better performance and optimization for large object reads and writes as compared to the default AWS connectors. See  [Stocator - Storage Connector for Apache Spark](https://github.com/CODAIT/stocator){: external}.

To access data objects in an {{site.data.keyword.cos_short}} bucket, use the following URI:
```
cos://<bucket_name>.<cos_instance_name>/<object_name>
```
where `<bucket>` is the {{site.data.keyword.cos_short}} bucket name, for example `b1`, `<cos_instance_name>` the identifier  that distinguishes between {{site.data.keyword.cos_short}} instances, for example `cosinstance1`, and `<object_name>` the data object to access, for example `object1`.

For example:
```
cos://b1.cosinstance1/object1
```

See the following examples of working with {{site.data.keyword.cos_short}} from {{site.data.keyword.iae_full_notm}}:

- [Uploading data to or downloading data from {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-data-movement-cos)

- [Running a Yarn application with data in {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-run-hadoop-jobs#running-wordcount-on-data-in-object-storage)

- [Accessing data in {{site.data.keyword.cos_short}} from Hive](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#accessing-data-in-ibm-cloud-object-storage-from-hive)

- [Importing tables from a database to {{site.data.keyword.cos_short}} using Sqoop](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-sqoop)

- [Running Oozie workflows with data in {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-oozie)

- [Submitting Spark applications from {{site.data.keyword.cos_short}} or applications that use data in {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api#submit-spark-applications-from-object-storage-or-on-data-in-object-stores)
