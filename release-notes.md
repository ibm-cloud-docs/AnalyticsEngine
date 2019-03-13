---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-06"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Release notes
{: #release-notes}

Use these notes to learn about the latest features, additions and changes to {{site.data.keyword.iae_full_notm}}.
{: shortdesc}
## {{site.data.keyword.iae_full_notm}} information

### 06 February

- The Ambari metrics service (AMS) is now available on all the packages, including the Spark and Hive packages. Previously it was available only on the Hadoop package. This service enables you to see metrics like CPU, memory, and so on, which assists you while troubleshooting the cluster or an application.

- Spark dynamic allocation is now enabled by default. The default behavior until now was two executors per Spark application, requiring you to tune cluster resources depending on your application's needs. This behavior has been changed by enabling Spark dynamic allocation, which allows a Spark application to fully utilize the cluster resources. See [Kernel settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#kernel-settings).

### 30 January 2019

- {{site.data.keyword.iae_full_notm}} now meets the required IBM controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. See [HIPAA readiness](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-hipaa).

### 24 January 2019

- Added support for impersonating the user of the Livy APIs by enabling the user to set proxyUser to clsadmin. See [Livy API](https://{DomainName}/docs/services/AnalyticsEngine?topic=AnalyticsEngine-livy-api).
- The default Python environment is now Python 3. Earlier default Spark Python runtime was `Python 2`. See [Installed libraries](https://{DomainName}/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).
- Added the Ambari configuration changes to the custom spark-default.conf file for Python 3 support. See [Installed libraries](https://{DomainName}/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).
- Scala and R user installation library directory structure was changed to `~/scala` for Scala and `~/R` for R.


### 30 November 2018

- You can now track life cycle management actions performed on the cluster by users and applications that have access to your service instance. See [Activity Tracker](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-at-tracker).

- To view the password of a cluster, a user must have either Writer or Manager IAM role on the {{site.data.keyword.iae_full_notm}} service instance.

### 09 November 2018

 - {{site.data.keyword.iae_full_notm}} is now also available in the region of Japan, in addition to the US-South, Germany, and United Kingdom regions.

  - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Japan is `https://api.jp-tok.ae.cloud.ibm.com`

  - An {{site.data.keyword.iae_full_notm}} cluster created in the region of Japan has the following format:

    ```<clustername>.jp-tok.ae.appdomain.cloud```

    For example:

    `https://xxxxx-mn001.jp-tok.ae.appdomain.cloud:9443`


### 24 September 2018

 - {{site.data.keyword.iae_full_notm}} is now available in a new region namely Germany, in addition to US-South and the United Kingdom regions.
 - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Germany is

    -	eu-de: `https://api.eu-de.ae.cloud.ibm.com`

- The {{site.data.keyword.iae_full_notm}} cluster is now created using  a new domain name to align with {{site.data.keyword.Bluemix_short}} domain name standards. It has the following format:

 ``` <clustername>.<region>.ae.appdomain.cloud```

  where region is us-south, eu-gb, or eu-de.

  For example, for Germany:  
  `https://xxxxx-mn001.eu-de.ae.appdomain.cloud:9443`

  **Note:** Old clusters can still exist and will function using the old cluster name format.
- Broken HBase quick links are fixed on the Ambari UI.
- Broken stderr/stdout links are fixed for Spark History on the Ambari UI.


### 14 September 2018

 - Support for advanced provisioning options to customize Ambari component configurations at the time the {{site.data.keyword.iae_full_notm}} service instance is created was added. See [Advanced provisioning options](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options).
 - The {{site.data.keyword.iae_full_notm}} REST API documentation can now be accessed at the following new location: `https://{DomainName}/apidocs/ibm-analytics-engine`
 - The {{site.data.keyword.iae_full_notm}} REST API endpoints have a new domain suffix:
  - us-south: `https://api.us-south.ae.cloud.ibm.com`
  - eu-gb: `https://api.eu-gb.ae.cloud.ibm.com`

   The older endpoints `https://api.dataplatform.ibm.com` and `https://api.eu-gb.dataplatform.ibm.com` are deprecated and will no longer be supported after the end of September 2018. You can create  new service credentials from the {{site.data.keyword.Bluemix_short}} console to fetch the new endpoint.

- You can set up cron jobs on the mn003 node (management slave 2).

 **Restriction**: You can set this up only on the mn003 node.
- Support for Spark SQL JDBC endpoint was added. See [Working with Spark SQL to query data](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-sql).
- The Stocator library was updated to 1.0.24.
- Broken Spark History links were fixed.
- Changes were made at the backend networking level for optimized performance of Cloud Object Storage workloads.
- Documented more examples for using HBase and Parquet-Hive. See [Working with HBase](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-hbase#moving-data-between-the-cluster-and-cloud-object-storage) and [Working with Hive](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#parquet).
- Added the {{site.data.keyword.iae_full_notm}} security model. See [Security model](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-security-model).
- Documented best practices for creating and maintaining a stateless cluster. See [Best practices](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-best-practices).


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
* You can use external MySQL to store Hive metadata. For details see [Externalizing the Hive Metastore to Compose for MySQL](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-the-hive-metastore-to-ibm-compose-for-mysql).
* Spark lazy initialization for Scala is supported, in addition to existing support for R and Python. For details see [Lazy Spark initialization](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-/lazy-spark-ini).
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
* You can resize clusters using the Cluster Management user interface and the REST API. For more details, see [Resizing clusters](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-resize-clusters).
