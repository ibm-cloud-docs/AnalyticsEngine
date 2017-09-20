---

copyright:
  years: 2017
lastupdated: "2017-09-19"

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
