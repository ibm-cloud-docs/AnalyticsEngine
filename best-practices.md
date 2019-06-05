---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Best practices
{: #best-practices}

You should use the {{site.data.keyword.iae_full_notm}} cluster as a compute-only engine. Ideally, you should not store any data on the cluster; you should try to keep the cluster as stateless as possible. This deployment model is recommended so that you can delete and create clusters often to either save on costs, pick up new features, or work with new packages.

The following types of data stored on a cluster contribute towards making the cluster stateful and should preferably not be stored on the cluster:
- Data you want to analyze, and any results of your analysis
- Metadata
- Jobs with Spark, Hadoop, or Hive queries
- Any customization, for example, Spark or Hadoop configuration settings, or Python, R, Scala, or Java libraries.

To help you create and maintain a stateless cluster, you should try to keep to the following recommended best practices. The best practices include choosing the correct plan and selecting the appropriate configuration options:

- [Separate compute from storage](#separate-compute-from-storage)
- [Choose the right Cloud Object Storage configuration](#encryption)
  - [Disaster Recovery Resiliency](#DR-resiliency)
  - [Encryption](#cos-encryption)
  - [Cloud Object Storage credentials](#cos-credentials)
  - [Private endpoint for Cloud Object Storage](#private-endpoint)
- [Create a new cluster for new features or packages](#new-packages)
- [Customize cluster creation using scripts](#use-scripts)
- [Size the cluster appropriately](#cluster-size)
- [Choose the right plan](#plan)
- [Choose the appropriate hardware configuration](#hardware)
- [Choose the appropriate software package](#software)
- [Tune kernel settings for Spark interactive jobs](#tune-kernel-for-spark-interactive)
- [Store temporary files on cluster prudently](#store-temp-files)
- [Switch regions for disaster recovery](#disaster-recovery)

## Separate compute from storage
{: #separate-compute-from-storage}

Although the {{site.data.keyword.iae_full_notm}} cluster includes the Hadoop component with HDFS running on the compute nodes, you should use IBM Cloud Object Storage as the primary data store. You should use the HDFS nodes only as a data store for sandbox-type workloads.

{{site.data.keyword.iae_full_notm}} can be configured to work with [data in IBM Cloud Object Storage](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos) with [Hive table metadata stored in a Compose for MySQL service](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-the-hive-metastore-to-ibm-compose-for-mysql), which resides outside of the cluster. When jobs are executed, they run on the compute nodes by bringing in data (as required by the job plan) from Cloud Object Storage. <!-- For more on this topic refer to this [{{site.data.keyword.iae_full_notm}}  whitepaper](https://www-01.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=ASW12451USEN&). --> Note that the application binaries can reside in Cloud Object Storage as well.

![Shows separating compute from storage in the {{site.data.keyword.iae_full_notm}} cluster.](images/SeparateComputeFromStorage.png)

## Choose the right Cloud Object Storage configuration
{: #encryption}

Consider the following configuration aspects:

### Disaster Recovery (DR) Resiliency
{: #DR-resiliency}

You should use the IBM COS Cross Regional resiliency option that backs up your data across several different cities in a region. In contrast, the Regional resiliency option back ups data in a single data center. See the [Cloud Object Storage documentation.](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-endpoints#endpoints)

### Encryption
{: #cos-encryption}

Cloud Object Storage comes with default built-in encryption. You can also configure Cloud Object Storage to work with the BYOK Key Protect service. See [here](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial) for more information. Note however that Key Protect is currently only supported for regional buckets. See the [Cloud Object Storage manage encryption](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-encryption#encryption) documentation.

### Cloud Object Storage credentials
{: #cos-credentials}

By default, Cloud Object Storage uses IAM-style credentials. If you want to work with AWS-style credentials, you need to provide the inline configuration parameter `{"HMAC":true}` as shown [here](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials).

### Private endpoint for Cloud Object storage
{: #private-endpoint}

Private endpoints provide better performance and do not incur charges for any outgoing or incoming bandwidth even if the traffic is across regions or across data centers. Whenever possible, you should use a private endpoint.

## Create a new cluster for new features or packages
{: #new-packages}

Upgrading components on the  {{site.data.keyword.iae_full_notm}} cluster to a higher version is not supported. If you want to include a new feature, a new package, or a fix, you should delete the old cluster and create a new one. The earlier reference to separating compute from storage will have a bearing on this best practice as well. If you separate the data from compute, then it is easier for you to delete an existing cluster and create a new one from where you can run your jobs again. This is also the recommended deployment pattern if you want your input and output data (as a result of the job execution) to be accessible even after the cluster is deleted.

## Customize cluster creation using scripts
{: #use-scripts}

To enable deleting and creating clusters often, you should use customization scripts to configure your cluster, and to install custom libraries and packages. This way, you won't have to manually customize the cluster every time you create a new one. See [Customizing a cluster](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-cust-cluster).

## Size the cluster appropriately
{: #cluster-size}

Size your cluster depending on your environment and workload:

 - For your development environment, create an {{site.data.keyword.iae_full_notm}} cluster  with 1 Management and 2 compute nodes
 - For your staging environment, the cluster size depends on the workloads and job characteristics, as well as the service-level agreement (SLA).
 - For Production environment, the cluster size depends on the workloads and job characteristics, and your SLA. Contact IBM Sales to get suitable sizing for your requirements.

## Choose the right plan
{: #plan}

Select the plan based on your workload use-case:
 - For deploy, run and discard use-cases, select hourly plan clusters.
 - For long running clusters, select monthly plan clusters.

## Choose the appropriate hardware configuration
{: #hardware}

For running parallel jobs, choose the memory-intensive node size. For example, if the number of concurrent notebooks (connected from IBM Watson Studio to {{site.data.keyword.iae_full_notm}}) is greater than 2, you should select the memory-intensive node size and not the default node size.

## Choose the appropriate software package
{: #software}

The software packages on `AE 1.2` clusters include components for Horton Dataworks Platform 3.1 and on `AE 1.1` clusters for Horton Dataworks Platform 2.6.5.

| AE 1.2  clusters     | Based on HDP 3.1        |
|-----------------|-----------------------------|
| `AE 1.2 Hive LLAP` <br>Choose if you are planning to run Hive in interactive mode, with preconfigured settings for Hive LLAP for faster responses. | Hadoop, Livy, Knox, Ambari, Anaconda-Py, Hive (LLAP mode) |
| `AE 1.2 Spark and Hive` <br>Choose if you are planning to run Hive and/or Spark workloads.  | Hadoop, Livy, Knox, Spark, JEG, Ambari, Anaconda Py, Hive (non LLAP mode ) |
| `AE 1.2 Spark and Hadoop`<br>Choose if you are planning to run Hadoop workloads in addition to Spark workloads. | (AE 1.2 Spark and Hive) +  HBase, Phoenix, Oozie |

**Note:**  Currently you cannot resize a cluster that uses the `AE 1.2 Hive LLAP` software package.

| AE 1.1 clusters      | Based on HDP 2.6.5        |
|-----------------|-------------------------------|
| `AE 1.1 Spark` <br>Choose if you are planning to run only Spark workloads. | Spark, Hadoop, Jupyter Enterprise, Livy, Knox, Ambari, Anaconda-Py |
| `AE 1.1 Spark and Hive` <br>Choose if you are planning to run Hive and/or  Spark workloads.| (AE 1.1 Spark) + Hive |
| `AE 1.1 Spark and Hadoop`<br>Choose if you are planning to run Hadoop workloads in addition to Spark workloads. | (AE 1.1 Spark and Hive) + HBase, <br>Oozie, Flume, Phoenix |

**Note:** Python 2 is available only in `AE 1.1`. However, you are encouraged to write your applications in Python 3 as Python 2 will only be supported until the end of 2019. See [Installed libraries](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).

## Tune kernel settings for Spark interactive jobs
{: #tune-kernel-for-spark-interactive}

When running large Spark interactive jobs, you might need to adjust kernel settings to tune resource allocation. To get the maximum performance from your cluster for a Spark job, make sure the kernel settings for memory and executor are correct. See [Kernel settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings).

## Store temporary files on the cluster prudently
{: #store-temp-files}

Although you should use IBM Cloud Object Storage as your primary storage for all data files and job binaries, you might want to create and store some temporary data or files on the cluster itself. If you need to do that, you can store this data under the `/home/wce/clsadmin` directory on any of the nodes of the cluster. Note that you have about 20 GB capacity under `/home` across all the three management nodes. However, you should not use more than 80% of this total capacity so as to not disrupt the normal functioning on the cluster. You should avoid saving data under the `/tmp` directory because this space is used as the scratch directory for job execution.

Note that any data stored on the cluster is not persistent outside the cluster lifecycle. If the cluster is deleted, the data will be expunged too. So make sure you backup any important data you store on the cluster.

## Switch regions for disaster recovery
{: #disaster-recovery}

You can create {{site.data.keyword.iae_full_notm}} service instances in different regions, for example, in the US South, the United Kingdom, Germany, and Japan. In the event that you cannot create a service instance in one region, you can switch to an alternate region which hosts  {{site.data.keyword.iae_full_notm}}. You will not be able to access any existing clusters from the new region. However, creating a new cluster in a new region should not be a problem if you followed the recommended best practices described in this topic and kept your existing cluster as stateless as possible with data and jobs residing outside the cluster.

See the [list of supported regions and the endpoints to use](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE#creating-a-service-instance-using-the-ibm-cloud-command-line-interface) or refer to the {{site.data.keyword.Bluemix_short}} catalog for {{site.data.keyword.iae_full_notm}}.
