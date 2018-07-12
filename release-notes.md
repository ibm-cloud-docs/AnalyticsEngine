---

copyright:
  years: 2017,2018
lastupdated: "2018-06-06"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Release notes
Use these notes to learn about the latest features, additions and changes to {{site.data.keyword.iae_full_notm}}.
{: shortdesc}
## {{site.data.keyword.iae_full_notm}} information

### 6 June 2018
 - You can no longer provision {{site.data.keyword.iae_full_notm}} service instances using the Cloud Foundry CLI. You must use the {{site.data.keyword.Bluemix_short}} CLI.
 - The following software packages were added:
   - **AE 1.1 Spark**
   - **AE 1.1 Spark and Hadoop**
   - **AE 1.1 Spark and Hive**

  These packages are based on Hortonworks Data Platform 2.6.5 and  include Spark 2.3.



### 26 March 2018
  -	{{site.data.keyword.iae_full_notm}} service instances can now also be deployed in the United Kingdom region
  - Support for Resource Controller APIs and CLI tools to create an {{site.data.keyword.iae_full_notm}} service instance has been added. Resource controller based service instances help you to provide fine grained access control to other users when you want to share your service instance.
  - The software pack **AE 1.0 Spark and Hive** has been introduced. Choose this pack if you intend to work with Hive and/or Spark work loads and do not require other Hadoop components offered in the **AE 1.0 Spark and Hadoop** pack.



### 12 January 2018

- {{site.data.keyword.iae_full_notm}} now supports Apache Phoenix 4.7. With this support, you can query HBase through SQL.
- Jupyter Enterprise Gateway 0.8.0 was added

### 1 November 2017 GA
- {{site.data.keyword.iae_full_notm}} offers the following new service plans:
  - Lite
  - Standard-Hourly
  - Standard-Monthly

  The Beta Standard plan is no longer supported.

- You can now select to configure a memory intensive hardware type

### 23 October 2017

- The ibmos2spark (0.0.9) package for Scala and the imbos2spark (1.0.1) package for Python were updated to support the insert to code feature in {{site.data.keyword.DSX_short}}
- Jupyter Enterprise Gateway support 0.5.0 was added
- Stocator 1.0.10 support for COS IAM using Spark was added
- The Hortonworks Data Platform build was moved from non-GA (HDP-2.6.2.0-198) to GA (HDP-2.6.2.0-205)
- Support of a Spark job progress bar was added to {{site.data.keyword.iae_full_notm}}. This enables monitoring Spark job progress in notebooks run in {{site.data.keyword.DSX_short}}.
- The Spark History UI fix for downloading Spark job logs (KNOX Issue) was added

### 19 September 2017

* The {{site.data.keyword.iae_full_notm}} service is in beta program
* {{site.data.keyword.iae_full_notm}} is now based on Hortonworks Data Platform (HDP 2.6)
* You can seamlessly integrate {{site.data.keyword.iae_full_notm}} from {{site.data.keyword.DSX_short}}.
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

* The {{site.data.keyword.iae_full_notm}} service is classified as Experimental and is available only under the [Experimental catalog](https://console.bluemix.net/catalog/labs?env_id=ibm:yp:us-south).
* You can resize clusters using the Cluster Management user interface and the REST API. For more details, see [Resizing clusters](./Resize-clusters.html#resizing-clusters).
