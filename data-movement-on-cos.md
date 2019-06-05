---
copyright:
  years: 2017, 2019
lastupdated: "2019-05-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Data movement on {{site.data.keyword.cos_full_notm}}
{: #data-movement-cos}

You can upload data to one of the following sources, or download data from {{site.data.keyword.cos_full_notm}} to one of the following destinations:

-	HDFS file system of your cluster
-	Local file system of your cluster
-	Outside your cluster

## Operations on the cluster
From the management node or a data node of your cluster (for example, `mn003` or `dn001`), you can copy, view, list, and perform any other basic file system operation on {{site.data.keyword.cos_short}}. For example:

- To copy files from the cluster’s local file system to {{site.data.keyword.cos_short}} use:
```
hdfs dfs –copyFromLocal /tmp/testfile cos://mybucket.myprodservice/
hdfs dfs –put /tmp/myfile2 cos://mybucket.myprodservice/```

- To copy files from {{site.data.keyword.cos_short}} to the cluster’s local file system, use:
```
hdfs dfs –get cos://mybucket.myprodservice/myfile2```

- Other useful housekeeping commands (list, view, make dir, remove) from cluster include:
```
hdfs dfs –ls cos://mybucket.myprodservice/myfile1
hdfs dfs –cat cos://mybucket.myprodservice/myfile1
hdfs dfs –mkdir cos://mybucket.myprodservice
hdfs dfs –rm cos://mybucket.myprodservice/myfile1```

- To copy files between HFDS and {{site.data.keyword.cos_short}} using `distcp`, use:
```
hadoop distcp /tmp/test.data  cos://mybucket.myprodservice/mydir/
hadoop distcp cos://mybucket.myprodservice/mydir/ /tmp/test.data```

  `hdfs://` is implied. It can also be explicitly specified, if the {{site.data.keyword.Bluemix_short}} hosting location is `us-south` for example:
```
hdfs://chs-czq-182-mn002.us-south.ae.appdomain.cloud:8020/tmp/test.data```

## Operations outside the cluster

For information on how you can use the {{site.data.keyword.cos_short}}  API or the UI to work with data objects outside of your cluster, refer to the [{{site.data.keyword.cos_short}} documentation](/docs/services/cloud-object-storage?topic=cloud-object-storage-about#about).
