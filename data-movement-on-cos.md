copyright:
  years: 2017,2018
lastupdated: "2017-12-14"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Data movement on IBM COS S3

This sections shows you to upload to one of the following sources or download data from IBM COS S3 to one of the following destinations:

-	HDFS File system of your cluster
-	Local Filesystem of your cluster
-	Outside your cluster

## Operations on the cluster
From the management node or a data node of your cluster (for example, `mn003` or `dn001`), you can copy, view, list, and perform any other basic file system operation on the object store. For example:

- To copy files from the cluster’s local file system to the object store, use:
```
hdfs dfs –copyFromLocal /tmp/testfile cos://mybucket.myprodservice/
hdfs dfs –put /tmp/myfile2 cos://mybucket.myprodservice/```

- To copy files from the object store to the cluster’s local file system, use:
```
hdfs dfs –get cos://mybucket.myprodservice/myfile2```

- Other useful housekeeping commands (list, view, make dir, remove) from cluster include:
```
hdfs dfs –ls cos://mybucket.myprodservice/myfile1
hdfs dfs –cat cos://mybucket.myprodservice/myfile1
hdfs dfs –mkdir cos://mybucket.myprodservice
hdfs dfs –rm cos://mybucket.myprodservice/myfile1```

- To copy files between HFDS and the object store using `distcp`, use:
```
hadoop distcp /tmp/test.data  cos://mybucket.myprodservice/mydir/
hadoop distcp cos://mybucket.myprodservice/mydir/ /tmp/test.data```

  `hdfs://`` is implied. It can also be explicitly specified, for example:
```
hdfs://chs-czq-182-mn002.bi.services.us-south.bluemix.net:8020/tmp/test.data```

## Operations outside the cluster

For information on how you can use the IBM COS S3 API or the UI to work with data objects outside of your cluster, refer to the [Cloud Object Storage documentation](https://console.bluemix.net/docs/services/cloud-object-storage/index.html).
