---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-04"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Release notes
{: #release-notes}

Use these notes to learn about the latest features, additions and changes to {{site.data.keyword.iae_full_notm}}.
{: shortdesc}

## {{site.data.keyword.iae_full_notm}} information


### 12 January 2021

- New security patches for **AE-1.2.v29.2**

  **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.2**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

  Customers are required to review the following CVE information and take appropriate action:  
  - [CVE-2020-25659](https://bugzilla.redhat.com/show_bug.cgi?id=1889988): the python-cryptography package was upgraded to version 3.3
  - [CVE-2020-28928](https://nvd.nist.gov/vuln/detail/CVE-2020-28928)
  - [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971)
  - [CVE-2020-27350](http://www.ubuntu.com/usn/usn-4667-1)

  Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

### 07 December 2020

- **[AE-1.2.v29.1]**

  - A fix was added that makes the Hive Metastore transparently recover from PostgreSQL metastore database restarts or network interruption issues. If you are externalizing the Hive metastore to IBM Cloud Databases for PostgreSQL, you need to set the socketTimeout value in the JDBC URL. See [Externalizing the Hive metastore to Databases for PostgreSQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-hive-metastore).
  -  Another fix was added so that you no longer need to explicitly grant permissions for the PostgreSQL certificate that you place in `/home/common/wce/clsadmin/` on `mn002`. The folder now has the required permissions and the permissions are retained through restarts.


### 17 October 2020

- New security patches for **AE-1.2.v29**

  **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

  Customers are required to review the following CVE information and take appropriate action:

  - [CVE-2020-26116](https://exchange.xforce.ibmcloud.com/vulnerabilities/189404)
  - [CVE-2019-20907](https://exchange.xforce.ibmcloud.com/vulnerabilities/185442)
  - [CVE-2020-14422](https://exchange.xforce.ibmcloud.com/vulnerabilities/184320)

  To apply these fixes, the cluster Python runtime version was upgraded from Python3.7.1-Anaconda3 (conda 4.5.12) distribution to Python3.7.9-Miniconda3.7 (conda 4.8.5) distribution. Note that this implies that the provided Python packages that were present in the earlier Anaconda version will no longer be available out of the box in newly created clusters. If you need to use any of the old packages you need to install the packages explicitly by using the `pip install` command. Remember to follow the recommendations for creating and deleting clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

### 01 October 2020

- A fix was added to alleviate cluster instability issues caused by an error in an underlying Docker runtime. If you created a cluster between 23 July 2020 and 01 October 2020, you might have experienced intermittent instability that manifested as connectivity issues or down times in nodes. With today's deployment, the runtime version has been replaced by an earlier stable runtime version.

  We strongly urge you to create new clusters and discard all  clusters created between 23 July 2020 and 01 October 2020. If for any reason, you need to retain an old cluster, contact support and request for an in-place fix. Note that this in-place fix will require a disruptive restart of the cluster.

  In general, you should following the recommendations for creating and deleting clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

### 17 September 2020

- **[AE-1.2.v28.5]** A new stocator version was released with fixes for the [Spark Dynamic Partition Insert issue](https://github.com/CODAIT/stocator/issues/256) and [Ceph Storage list objects issue](https://github.com/CODAIT/stocator/issues/252).

### 10 September 2020

- New security patch for **AE-1.2.v28.4**

  The following patch is available for security vulnerabilities on the underlying VMs for IBM Java.

  **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patch, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v28.4**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

  Customers are required to review the following CVE information and take appropriate action:
  - [CVE-2019-17639](https://www.ibm.com/blogs/psirt/security-bulletin-multiple-vulnerabilities-may-affect-ibm-sdk-java-technology-edition-3/)
- You can now configure log aggregation for the HDFS component. See [Configuring log aggregation](/docs/AnalyticsEngine?topic=AnalyticsEngine-log-aggregation).
- A fix was added that prevents HDFS audit logs from filling up  disk space, which was caused by a misconfiguration of the log4j rotation property that disrupted the way clusters should work.
- You can now use the time series library in your Spark applications, which provides a rich time series data model and  imputation functions for transforming, reducing, segmenting, joining, and forecasting time series. SQL extensions to time series are also provided. See [Time series library](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series).

### 20 August 2020

- As of this deployment, Analytics Engine cluster’s build version will be available in `/home/common/aeversion.txt` on the nodes of the cluster. You can check this file after you SSH to the cluster. This will help in tracking fixes that were made available against a particular version of Analytics Engine. For example, this deployment version is AE-1.2.v28.3.

- New security patches for **AE-1.2.v28.3**

  The following patches are available for the security vulnerabilities on the OS level packages of cluster nodes as well as for config vulnerabilities at the OS level.

  **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v28.3**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

  Customers are required to review the following CVE information and take appropriate action:
  - [CVE-2019-11729](https://access.redhat.com/errata/RHSA-2019:4190)
  -[CVE-2019-11745](https://access.redhat.com/errata/RHSA-2019:4190)
  - [CVE-2020-8616](https://access.redhat.com/errata/RHSA-2020:2344)
  - [CVE-2020-8617](https://access.redhat.com/errata/RHSA-2020:2344)
  - [CVE-2019-12735](https://access.redhat.com/errata/RHSA-2019:1619)
  - [CVE-2020-11008](https://access.redhat.com/errata/RHSA-2020:2337)
  - [CVE-2020-12049](https://access.redhat.com/errata/RHSA-2020:2894)
  - [CVE-2020-1967](https://gitlab.alpinelinux.org/alpine/aports/-/issues/11429)
  - [CVE-2020-3810](https://ubuntu.com/security/notices/USN-4359-1)
  - [CVE-2019-5188](http://www.ubuntu.com/usn/usn-4249-1)
  - [CVE-2019-5094](http://www.ubuntu.com/usn/usn-4142-1)
  - [usn-4038-3](http://www.ubuntu.com/usn/usn-4038-3)
  - [CVE-2017-12133](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2017-18269](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2018-11236](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2018-11237](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2018-19591](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2018-6485](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2019-19126](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2019-9169](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2020-10029](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2020-1751](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2020-1752](http://www.ubuntu.com/usn/usn-4416-1)
  - [CVE-2019-13627](http://www.ubuntu.com/usn/usn-4236-2)
  - [CVE-2018-16888](http://www.ubuntu.com/usn/usn-4269-1)
  - [CVE-2019-20386](http://www.ubuntu.com/usn/usn-4269-1)
  - [CVE-2019-3843](http://www.ubuntu.com/usn/usn-4269-1)
  - [CVE-2019-3844](http://www.ubuntu.com/usn/usn-4269-1)
  - [CVE-2020-1712](http://www.ubuntu.com/usn/usn-4269-1)
  - [CVE-2016-9840](http://www.ubuntu.com/usn/usn-4246-1)
  - [CVE-2016-9841](http://www.ubuntu.com/usn/usn-4246-1)
  - [CVE-2016-9842](http://www.ubuntu.com/usn/usn-4246-1)
  - [CVE-2016-9843](http://www.ubuntu.com/usn/usn-4246-1)
  - [CVE-2019-9924](http://www.ubuntu.com/usn/usn-4058-1)

### 23 July 2020

- Default values for the `gateway.socket.*` parameters were added to fix errors related to the length of messages when using Spark WebSockets.
- Docker on the underlying Host VMs was upgraded as part of the security fixes that were applied for [CVE-2020-13401](https://exchange.xforce.ibmcloud.com/vulnerabilities/182750).

### 22 May 2020

- You can now whitelist access to a private endpoint cluster. See [Whitelisting IP addresses to control network traffic](/docs/AnalyticsEngine?topic=AnalyticsEngine-allowlist-to-cluster-access).
- You can also now use the {{site.data.keyword.iae_full_notm}} Java SDK to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API. See [Using Java](/docs/AnalyticsEngine?topic=AnalyticsEngine-java).

### 17 May 2020

- Security patches to Spark CLI were applied.

### 12 May 2020

- The underlying HDP stack for `AE 1.2` was changed from 3.1.0.0-78 to 3.1.5.0-152.
- If you associate the same IBM Cloud Databases for PostgreSQL instance with all of your {{site.data.keyword.iae_full_notm}} clusters and you create new clusters after 12 May 2020, you must run the following command on the `mn002` node of the clusters to continue working with the dabasebase instance:

  ```
  /usr/hdp/current/hive-server2/bin/schematool -url 'jdbc:postgresql://<YOUR-POSTGRES-INSTANCE-HOSTNAME>:<PORT>/ibmclouddb?sslmode=verify-ca&sslrootcert=<PATH/TO/POSTGRES/CERT>' -dbType postgres -userName <USERNAME> -passWord <PASSWORD> -upgradeSchema 3.1.1000 -verbose
  ```

  The reason is a database schema version change. You do not have to issue this command if you associate a new IBM Cloud Databases for PostgreSQL instance with the {{site.data.keyword.iae_full_notm}} clusters.
- Starting from this release, Spark and Hive share the same metadata store.
- Security updates to JDK and WLP were applied on all host virtual machines. Also security updates to JDK on the cluster were installed.
- Resizing clusters by using the UI or the API fails for clusters that were created before 12 May 2020. The reason is an unexpected incompatibility in the Ambari version between HDP 3.1 (used in clusters created before 12 May 2020) and HDP 3.1.5 (used in clusters created after 12 May 2020). The problem does not affect clusters that were created after 12 May 2020. Those clusters can be resized.

### 05 May 2020

- Support for the {{site.data.keyword.iae_full_notm}} Go SDK is now available which you can use to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API. See [Using the {{site.data.keyword.iae_full_notm}} Go SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-go).


### 21 April 2020

- You can now use the {{site.data.keyword.iae_full_notm}} Node.js and Python SDK to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API. See:

  - For Node.js: [Using the {{site.data.keyword.iae_full_notm}} Node.js SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-node-js)
  - For Python: [Using the {{site.data.keyword.iae_full_notm}} Python SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-python-sdk)

### 03 April 2020

- Memory tuning enhancements were deployed for standard (default) hardware clusters. If you have standard hardware clusters that you created before 03 April 2020, you need to delete those clusters and create new ones to avoid running into out memory issues or having unresponsive clusters that need to be rebooted by the IBM Support team.

### 02 April 2020

- You can now use data skipping libraries to boost the performance of Spark SQL queries by associating a summary metadata with each data object. This metadata is used during query evaluation to skip over objects which have no relevant data. See [Data skipping for Spark SQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-data-skipping).

### 13 February 2020

- Java on clusters was updated to Open JDK.

### 12 February 2020

- You can enable encryption for data in transit for Spark jobs by explicitly configuring the cluster using a combination of Advanced Options and customization.  See [Enabling Spark jobs encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-encryption).

### 16 January 2020

- Security updates to JDK and WLP were applied on all host virtual machines.

### 14 January 2020

- You can now work with the geospatio-temporal library to expand your data science analysis in Python notebooks to include location analytics by gathering, manipulating and displaying imagery, GPS, satellite photography and historical data. The documentation includes  sample code snippets of many useful functions and links to sample notebooks in the IBM Watson Studio Gallery. See [Working with the spatio-temporal library](/docs/AnalyticsEngine?topic=AnalyticsEngine-geospatial-geotemporal-lib).
- Updates were made to best practices around choosing the right Databases for PostgreSQL configuration, configuring the cluster for log monitoring and troubleshooting and using private endpoints. See [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

### 16 December 2019

- The way that you can use the {{site.data.keyword.iae_full_notm}} Lite plan has changed. The Lite plan is now available only to institutions that have signed up with IBM to try out the Lite plan. See [How to use the Lite plan](/docs/AnalyticsEngine?topic=AnalyticsEngine-general-faqs#lite-plan).
- You can now use adhoc PostgreSQL customization scripts to configure the cluster to work with PostgreSQL. See [Running an adhoc customization script for configuring Hive with a Postgres external metastore](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-examples#postgres-metastore).
- {{site.data.keyword.iae_full_notm}} now supports Parquet modular encryption that allows encrypting sensitive columns when writing Parquet files and decrypting these columns when reading the encrypted files. Besides ensuring privacy, Parquet encryption also protects the integrity of stored data by detecting any tampering with file contents. See [Working with Parquet encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-parquet-encryption).

### 25 November 2019

- You can now use the {{site.data.keyword.Bluemix_notm}} service endpoints feature to securely access your {{site.data.keyword.iae_full_notm}} service instances over the {{site.data.keyword.Bluemix_notm}} private network. You can choose to use either public or private endpoints for the {{site.data.keyword.iae_full_notm}} service. The choice needs to be made at the time you provision the instance. See [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).
- Fixed several broken links in Spark history server and Yarn applications.
- Fixed broken Livy sessions API, benign timeline service errors, tuned Knox for timeout errors.


### 07 October 2019

- IBM Cloud Databases for PostgreSQL is now available for externalizing Hive cluster metadata. To learn how to configure your cluster to store Hive metadata in PostgreSQL, see [Externalizing the Hive metastore](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-hive-metastore).

 You should stop using Compose For MySQL as the Hive metastore.
- Hive View has been removed from the underlying platform in `AE 1.2`. You can use any other JDBC UI based client such as SQuirrel SQL or Eclipse Data Source Explorer instead.


### 24 September 2019

- `AE 1.1` (based on Hortonworks Data Platform 2.6.5) is deprecated. You can no longer provision `AE 1.1` clusters. You also cannot resize and add additional nodes to an `AE 1.1` cluster.

 Although existing clusters will still continue to work and be supported until December 31, 2019,  you should stop using those now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

 All provisioned instances of AE 1.1 will be deleted after December 31, 2019. See the [deprecation notice](https://www.ibm.com/cloud/blog/announcements/deprecation-of-ibm-analytics-engine-v1-1){: external}.

### 21 August 2019

- To enhance log analysis, log monitoring, and troubleshooting, {{site.data.keyword.iae_full_notm}} now supports aggregating cluster logs and job logs to a centralized logDNA server of your choice. See details in [Configuring log aggregation](/docs/AnalyticsEngine?topic=AnalyticsEngine-log-aggregation).

 Note that log aggregation can only be configured for  {{site.data.keyword.iae_full_notm}} clusters created on or after  August 21, 2019.



### 15 May 2019

- A new version of {{site.data.keyword.iae_full_notm}} is now available: `AE  1.2` based on HDP 3.1. It has 3 software packages:

   - `AE 1.2 Hive LLAP`
   - `AE 1.2 Spark and Hive`
   - `AE 1.2 Spark and Hadoop`

 See [Provisioning {{site.data.keyword.iae_full_notm}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE).
- `AE 1.0` (based on HDP 2.6.2) is deprecated. You can no longer provision `AE 1.0` clusters. You also cannot resize and add additional nodes to an `AE 1.0` cluster.

 Although existing clusters will still continue to work and be supported until September 30, 2019, you should stop using those now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

 All provisioned instances of AE 1.0 will be deleted after September 30, 2019.
- A new software package `AE 1.2 Hive LLAP` was added to `AE 1.2`, which supports realtime interactive queries. Note however that currently you cannot resize a cluster created using this package.
- `AE 1.2` supports Python 3.7. Although `AE 1.1` still supports both Python 3.5 and Python 2.7, you should start moving your Python-based applications or code to Python 3.7 now. Especially considering that the open source community has announced the end of support for Python 2.
- `AE 1.2` does not support HDFS encryption zones. To store sensitive data with encryption requirements, select the appropriate COS encryption options. See details in [Best practices](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#cos-encryption).

### 18 March 2019

- The cluster credentials can no longer be retrieved via the service endpoints. You can now only retrieve the cluster password by invoking the {{site.data.keyword.iae_full_notm}} [`reset_password`](/docs/AnalyticsEngine?topic=AnalyticsEngine-reset-cluster-password#reset-cluster-password) REST API and you must have the appropriate user permissions.
- The predefined environment variable `AMBARI_PASSWORD` is no longer available for use in a cluster customization script.

### 06 February 2019

- The Ambari metrics service (AMS) is now available on all the packages, including the Spark and Hive packages. Previously it was available only on the Hadoop package. This service enables you to see metrics like CPU, memory, and so on, which assists you while troubleshooting the cluster or an application.

- Spark dynamic allocation is now enabled by default. The default behavior until now was two executors per Spark application, requiring you to tune cluster resources depending on your application's needs. This behavior has been changed by enabling Spark dynamic allocation, which allows a Spark application to fully utilize the cluster resources. See [Kernel settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#kernel-settings).

### 30 January 2019

- {{site.data.keyword.iae_full_notm}} now meets the required IBM controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements. See [HIPAA readiness](/docs/AnalyticsEngine?topic=AnalyticsEngine-hipaa).

### 24 January 2019

- Added support for impersonating the user of the Livy APIs by enabling the user to set proxyUser to clsadmin. See [Livy API](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api).
- The default Python environment is now Python 3. Earlier default Spark Python runtime was `Python 2`. See [Installed libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).
- Added the Ambari configuration changes to the custom spark-default.conf file for Python 3 support. See [Installed libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).
- Scala and R user installation library directory structure was changed to `~/scala` for Scala and `~/R` for R.


### 30 November 2018

- You can now track life cycle management actions performed on the cluster by users and applications that have access to your service instance. See [Activity Tracker](/docs/AnalyticsEngine?topic=AnalyticsEngine-at_events).

- To view the password of a cluster, a user must have either Writer or Manager IAM role on the {{site.data.keyword.iae_full_notm}} service instance.

### 09 November 2018

- {{site.data.keyword.iae_full_notm}} is now also available in the region of Japan, in addition to the US-South, Germany, and United Kingdom regions.

  - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Japan is `https://api.jp-tok.ae.cloud.ibm.com`

  - An {{site.data.keyword.iae_full_notm}} cluster created in the region of Japan has the following format:

    ```
     <clustername>.jp-tok.ae.appdomain.cloud
    ```

    For example:

    `https://xxxxx-mn001.jp-tok.ae.appdomain.cloud:9443`


### 24 September 2018

 - {{site.data.keyword.iae_full_notm}} is now available in a new region namely Germany, in addition to US-South and the United Kingdom regions.
 - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Germany is

    -	eu-de: `https://api.eu-de.ae.cloud.ibm.com`

- The {{site.data.keyword.iae_full_notm}} cluster is now created using  a new domain name to align with {{site.data.keyword.Bluemix_short}} domain name standards. It has the following format:

 ```
 <clustername>.<region>.ae.appdomain.cloud
 ```

  where region is us-south, eu-gb, or eu-de.

  For example, for Germany:  
  `https://xxxxx-mn001.eu-de.ae.appdomain.cloud:9443`

  **Note:** Old clusters can still exist and will function using the old cluster name format.
- Broken HBase quick links are fixed on the Ambari UI.
- Broken stderr/stdout links are fixed for Spark History on the Ambari UI.


### 14 September 2018

 - Support for advanced provisioning options to customize Ambari component configurations at the time the {{site.data.keyword.iae_full_notm}} service instance is created was added. See [Advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options).
 - The {{site.data.keyword.iae_full_notm}} REST API documentation can now be accessed at the following new location: `https://{DomainName}/apidocs/ibm-analytics-engine`
 - The {{site.data.keyword.iae_full_notm}} REST API endpoints have a new domain suffix:
  - us-south: `https://api.us-south.ae.cloud.ibm.com`
  - eu-gb: `https://api.eu-gb.ae.cloud.ibm.com`

   The older endpoints `https://api.dataplatform.ibm.com` and `https://api.eu-gb.dataplatform.ibm.com` are deprecated and will no longer be supported after the end of September 2018. You can create  new service credentials from the {{site.data.keyword.Bluemix_short}} console to fetch the new endpoint.

- You can set up cron jobs on the mn003 node (management slave 2).

 **Restriction**: You can set this up only on the mn003 node.
- Support for Spark SQL JDBC endpoint was added. See [Working with Spark SQL to query data](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-sql).
- The Stocator library was updated to 1.0.24.
- Broken Spark History links were fixed.
- Changes were made at the backend networking level for optimized performance of Cloud Object Storage workloads.
- Documented more examples for using HBase and Parquet-Hive. See [Working with HBase](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hbase#moving-data-between-the-cluster-and-object-storage) and [Working with Hive](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#parquet).
- Added the {{site.data.keyword.iae_full_notm}} security model. See [Security model](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-model).
- Documented best practices for creating and maintaining a stateless cluster. See [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).


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
* You can use external MySQL to store Hive metadata. <!--For details see [Externalizing the Hive Metastore to Compose for MySQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-the-hive-metastore-to-ibm-compose-for-mysql).-->
* Spark lazy initialization for Scala is supported, in addition to existing support for R and Python. For details see [Lazy Spark initialization](/docs/AnalyticsEngine?topic=AnalyticsEngine-lazy-spark-ini).
* You can use IAM for both authentication and authorization of Cluster Manager UI

What has changed:
* During beta only ‘standard’ plan is supported
* Software package names have changed. The new values are:
  * AE 1.0 Spark (ae-1.0-spark) for Spark workloads
  * AE 1.0 Hadoop and Spark (ae-1.0-hadoop-spark) for Hadoop and Spark workloads.
  * For details about new package names and plan name refer <link to “how to provision”>
* Openstack “swift” protocol is not supported. Only s3 API is supported with IBM Cloud Object Storage.  

### 08 August 2017

* The {{site.data.keyword.iae_full_notm}} service is classified as Experimental and is available only under the [Experimental catalog](https://cloud.ibm.com/catalog/labs?env_id=ibm:yp:us-south).
* You can resize clusters using the Cluster Management user interface and the REST API. For more details, see [Resizing clusters](/docs/AnalyticsEngine?topic=AnalyticsEngine-resize-clusters).
