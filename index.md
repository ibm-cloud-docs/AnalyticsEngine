---

copyright:
  years: 2017, 2019
lastupdated: "2019-12-03"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# IBM Analytics Engine overview
{: #IAE-overview}

With {{site.data.keyword.iae_full_notm}} you can create Apache Spark and Apache Hadoop clusters in minutes and customize these clusters by using scripts. You can work with data in IBM Cloud Object Storage, as well as integrate other IBM Watson services like {{site.data.keyword.DSX_short}} and Machine Learning.

You can define clusters based on your application's requirements,  choosing the appropriate software pack, version and size of the clusters.

You can deploy {{site.data.keyword.iae_full_notm}} service instances in the following regions: US South, United Kingdom, Germany, and Japan. The {{site.data.keyword.iae_full_notm}} service is deployed in a data center which is physically located in the chosen region.

- [Cluster architecture](#cluster-architecture)
- [Outbound and inbound access](#outbound-and-inbound-access)
- [Software packages](#software-packages)
- [Software components of the cluster](#software-components-of-the-cluster)
- [Hardware configuration](#hardware-configuration)
- [Operating system](#operating-system)
- [Best practices when creating clusters](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-best-practices)

## Cluster architecture

A cluster consists of a management instance and one or more compute instances. The management instance itself consists of three management nodes, which run in the management instance. Each of the compute nodes runs in a separate compute instance.

Note: You are billed only at the instance level. For more details on billing, see [{{site.data.keyword.iae_full_notm}}   Pricing](https://www.ibm.com/cloud/analytics-engine/pricing){: external}.

### Management nodes

The three management nodes include:
- The master management node (`mn001`)
- Two management slave nodes (`mn002` and `mn003`).

Ambari and all of the Hadoop and Spark components of the cluster run on the management nodes. To find out which components run on which of the management nodes, click the **hosts** link on the upper right corner of the Ambari UI. Drill down further to get to the listing of the components running on each node.

### Compute nodes

Compute nodes are where the execution of jobs happens. You define the number of compute nodes at the time of cluster creation. Each of the compute nodes is designated as `dn001`, `dn002` and so on.

### Summary of cluster nodes

The following cluster nodes exist:

- `mn001`: master management node
- `mn002`: management slave 1
- `mn003`: management slave 2
- `dn001`: compute node 1
- `dn002`: compute node 2

## Outbound and inbound access

Cluster services are made available through various endpoints as described in this [section](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints).

From the endpoint list, you can see that the following ports are open for inbound traffic:

-	**9443**: this is the Admin port.

 The Ambari UI console and APIs are exposed at port 9443 (`https://xxxxx-mn001.<region>.ae.appdomain.cloud:9443`).
-	**8443**: cluster services like Hive, Spark, Livy, Phoenix, and so on are made available for programmatic consumption through the Knox gateway on port 8443

-	**22**: the cluster itself is accessible via SSH at standard port 22.

 When you SSH to a cluster (as described [here](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH)) you essentially log in to `mn003`. Once you have logged in to `mn003`, you can SSH to the compute nodes (referred to as `dn001`, `dn002` etc) and to `mn002`.

For example, to log in to a cluster in the US-South region, as given in the endpoint listing, enter:
```
ssh clsadmin@chs-tnu-499-mn003.us-south.ae.appdomain.cloud
```

Once you are on `mn003`, enter the following to log in to `mn002`:
```
ssh clsadmin@chs-tnu-499-mn002
```

and to log in to `dn001` enter:

```
ssh clsadmin@chs-tnu-499-dn001
```

**Note:**
- You can't SSH to the master management node `mn001`.
- You can SSH to the compute nodes only from within other nodes of the cluster.
- Outbound traffic is open from all nodes.

![Shows the {{site.data.keyword.iae_full_notm}} cluster architecture.](images/AnalyticsEngineCluster.png)

## Software packages

The following software packages are available when you create a cluster based on Hortonworks Data Platform (HDP) 3.1:

| AE 1.2       | Based on HDP 3.1        |
|-----------------|-----------------------------|
| `AE 1.2 Hive LLAP`  | Hadoop, Livy, Knox, Ambari, <br>Anaconda-Py, Hive (LLAP mode) |
| `AE 1.2 Spark and Hive` | Hadoop, Livy, Knox, Spark, JEG, Ambari, <br>Anaconda Py, Hive (non LLAP mode ) |
| `AE 1.2 Spark and Hadoop` | (AE 1.2 Spark and Hive) +  HBase, Phoenix, <br>Oozie |

**Important:**

1. You can no longer provision new instances of {{site.data.keyword.iae_full_notm}} using the `AE 1.1` software packages (based on HDP 2.6.5).
2. Currently you cannot resize a cluster that uses the `AE 1.2 Hive LLAP` software package.

## Software components of the cluster
You can create a cluster based on Hortonworks Data Platform 3.1. The following software components are available for HDP 3.1. Refer to the previous section which lists the software packages to find out which components are available in the provided software packages.   

|  AE 1.2 (HDP 3.1)
|---------------------|
|  Apache Spark 2.3.2 |
|  Hadoop 3.1.1|
|  Apache Livy 0.5|
|  Knox 1.0.0|
|  Ambari 2.7.3|
|  Anaconda with Python 3.7.1 |
|  Jupyter Enterprise Gateway 0.8.0
|  HBase 2.0.2 |
|  Hive 3.1.0 |
|  Hive LLAP 3.1.0 |
|  Oozie 4.3.1 |
|  Tez 0.9.1 |
|  ZooKeeper 3.4.6 |
|  Pig 0.16.0 |
|  Sqoop 1.4.7 |
|  Apache Phoenix 5.0.0 |
|  YARN 3.1.1 |
|  MapReduce2 3.1.1 |

## Hardware configuration

{{site.data.keyword.iae_full_notm}} supports two node sizes for spinning up clusters.

**Size: Default Node**

| Node Type | vCPU | Memory | HDFS Disks |
|---------|------------|-----------|-----------|
| Master Node | 4| 16 GB | NA |
| Data Node | 4| 16 GB | 2 x 300 GB |

**Size: Memory Intensive Node**

| Node Type | vCPU | Memory | HDFS Disks |
|---------|------------|-----------|-----------|
| Master Node | 32| 128 GB | NA |
| Data Node | 32| 128 GB | 3 x 300 GB |

## Operating System
The operating system used is Cent OS 7.
