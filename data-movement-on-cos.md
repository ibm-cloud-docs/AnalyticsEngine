---
copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Uploading files to {{site.data.keyword.cos_full_notm}}
{: #data-movement-cos}

From the management node or a data node of your cluster (for example, `mn003` or `dn001`), you can copy, view, list, and perform any other basic file system operation on {{site.data.keyword.cos_short}}.

You can move data:

-	[Between the local file system of your cluster and {{site.data.keyword.cos_full_notm}}](#moving-data-between-the-local-file-system-of-your-cluster-and-ibm-cloud-object-storage)
-	[Between the HDFS file system of your cluster and {{site.data.keyword.cos_full_notm}}](#moving-data-between-hdfs-and-ibm-cloud-object-storage)
-	[Directly to {{site.data.keyword.cos_full_notm}} (outside the {{site.data.keyword.iae_full_notm}} cluster)](#cos-outside-cluster)

## Moving data between the local file system of your cluster and  {{site.data.keyword.cos_full_notm}}

You can move data to and from the local file system of your cluster and {{site.data.keyword.cos_full_notm}}. For example:

- To copy files from the cluster’s local file system to {{site.data.keyword.cos_short}} use the following HDFS command:
```
hdfs dfs –copyFromLocal /tmp/testfile cos://mybucket.myprodservice/
hdfs dfs –put /tmp/myfile2 cos://mybucket.myprodservice/
```

- To copy files from {{site.data.keyword.cos_short}} to the cluster’s local file system, use:
```
hdfs dfs –get cos://mybucket.myprodservice/myfile2
```

## Moving data between HDFS and {{site.data.keyword.cos_full_notm}}

You can move data to and from the HDFS file system of your cluster and {{site.data.keyword.cos_full_notm}}. For example:

- To copy files between HFDS and {{site.data.keyword.cos_short}} using `distcp`, enter the following command:
```
hadoop distcp /tmp/test.data  cos://mybucket.myprodservice/mydir/
hadoop distcp cos://mybucket.myprodservice/mydir/ /tmp/test.data
```

  `hdfs://` is implied. It can also be explicitly specified, if the {{site.data.keyword.Bluemix_short}} hosting location is `us-south` for example:
```
hdfs://chs-czq-182-mn002.us-south.ae.appdomain.cloud:8020/tmp/test.data
```

## Data operations outside the cluster
{: #cos-outside-cluster}

For information on how you can use the {{site.data.keyword.cos_short}} API or the UI to work with data objects outside of your cluster, see [Uploading data to  {{site.data.keyword.cos_short}}](/docs/cloud-object-storage?topic=cloud-object-storage-upload).

## Useful {{site.data.keyword.cos_short}} housekeeping commands

You can issue any of the following commands from your cluster to a {{site.data.keyword.cos_full_notm}} bucket to list, view, create or remove a directory:
```
hdfs dfs –ls cos://mybucket.myprodservice/myfile1
hdfs dfs –cat cos://mybucket.myprodservice/myfile1
hdfs dfs –mkdir cos://mybucket.myprodservice
hdfs dfs –rm cos://mybucket.myprodservice/myfile1
```
