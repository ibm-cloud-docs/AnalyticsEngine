---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Release notes
Use these notes to learn about the latest features, additions and changes to IBM Analytics Engine.
{: shortdesc}
## IBM Analytics Engine information

### 1 November 2017 GA
- IBM Analytics Engine offers the following new service plans:
  - Lite 
  - Standard-Hourly
  - Standard-Monthly

  The Beta Standard plan is no longer supported.

- You can now select to configure a memory intensive hardware type

### 23 Oct 2017

- The ibmos2spark (0.0.9) package for Scala and the imbos2spark (1.0.1) package for Python were updated to support the insert to code feature in IBM Data Science Experience (DSX)
- Jupyter Enterprise Gateway support 0.5.0 was added
- Stocator 1.0.10 support for COS IAM using Spark was added
- The Hortonworks Data Platform build was moved from non-GA (HDP-2.6.2.0-198) to GA (HDP-2.6.2.0-205)
- Support of a Spark job progress bar was added to IBM Analytics Engine. This enables monitoring Spark job progress in notebooks run in DSX.
- The Spark History UI fix for downloading Spark job logs (KNOX Issue) was added

### 19 September 2017

* The IBM Analytics Engine service is in beta program
* IBM Analytics Engine is now based on Hortonworks Data Platform (HDP 2.6)
* You can seamlessly integrate IBM Analytics Engine from DSX.
* You can use external MySQL to store Hive metadata. For details see [Externalizing the Hive Metastore to Compose for MySQL](./external-hive-metastore.html).
* Spark lazy initialization for Scala is supported, in addition to existing support for R and Python. For details see [Lazy Spark initialization](./lazy-spark-initialization.html).
* You can use IAM for both authentication and authorization of Cluster Manager UI

What has changed:
* During beta only ‘standard’ plan is supported
* Software package names have changed. The new values are:
  * AE 1.0 Spark (ae-1.0-spark) for Spark workloads
  * AE 1.0 Hadoop and Spark (ae-1.0-hadoop-spark) for Hadoop and Spark workloads.
  * For details about new package names and plan name refer <link to “how to provision”>
* Openstack “swift” protocol is not supported. Only s3 API is supported with IBM Cloud Object Storage.  

### 08 August 2017

* The IBM Analytics Engine service is classified as Experimental and is available only under the [Experimental catalog](https://console.bluemix.net/catalog/labs?env_id=ibm:yp:us-south).
* You can resize clusters using the Cluster Management user interface and the REST API. For more details, see [Resizing clusters](./Resize-clusters.html#resizing-clusters).
