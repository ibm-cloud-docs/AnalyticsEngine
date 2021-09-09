---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}
{:help: data-hd-content-type='help'}

# Known issues
{: #known-issues}

This topic lists known issues that have been identified or were reported by users in error reports.

## Viewing the known issues
{: #view_known_issues}
{: help}
{: support}

List of known issues in the release:

| Category | Problem | AE version | Workaround |
|---------|---------|------------|------------|
| UI | The Cluster Management user interface does not function well in Internet Explorer 11 | All versions | The Management user interface functions well in Chrome, Safari and Firefox. Use these browsers to access the user interface. |
| Resizing a cluster | Resizing clusters through the UI or by using the API fails for clusters that were created before 12 May  2020. The reason is an unexpected incompatibility in the Ambari version between HDP 3.1 (used in clusters created before 12 May 2020) and HDP 3.1.5 (used in clusters created after 12 May 2020). The problem does not affect clusters that were created after 12 May 2020. Those clusters can be resized.| AE 1.2 | There is no workaround for existing clusters. You must create new clusters and resize them as required. |
| URL to download Hive standalone JAR is broken| In Ambari, when you click **Services > Hive > Summary** and in **Quick Links**, you click **Jdbc Standalone Jar Download**, you will notice that the link is broken.| AE 1.2 | Download the JAR from the [Hortonworks repository](http://repo.hortonworks.com/content/repositories/releases/org/apache/hive/hive-jdbc/). Select the driver version that corresponds to Hive version used on your IBM Analytics Engine cluster. |
| Hive metastore externalization | You can't use advanced provisioning options to use Databases for PostgreSQL as the Hive metastore. For details on externalizing the Hive metastore, see [Externalizing to Databases for PostgreSQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-hive-metastore). | AE 1.2 | To  configure the cluster to use Databases for PostgreSQL as the Hive metastore, you can either use the Ambari UI or customize the cluster after it was created. See [Configuring a cluster to work with PostgreSQL](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#configuring-a-cluster-to-work-with-postgresql). |
| Yarn Application UI link to logs is broken | When you click **Yarn >  Quick Links > Resource Manager UI** in the Ambari UI and select your application on the Applications tab, you will notice that the link to the application logs at the bottom right of the page is broken. | AE 1.2   \n  Created before end November 2019 | SSH to the cluster and run `yarn logs --applicationId <appId>`|
| Spark History server UI - Stages | The URL to drill down into the stages of a Spark job is broken due to a bug in the Knox re-write rules. | AE 1.1 | Remove `amp%3B` from the stages URL. For example, replace the following broken re-write URL (the example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`): `https://chs-xxx-yyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxxx_yyyy/stages/stage?amp%3Battempt=0&id=2` by this workaround URL: `https://chs-yyy-yyyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxx_yyyy/stages/stage?attempt=0&id=2` . |
|Spark History Server UI - StdOut and StdErr logs | The drilldown links to StdOut and StdErr logs from the Stages or Executors tabs  are broken. |AE 1.2   \n  Created before end November 2019| SSH to the cluster and run `yarn logs --applicationId <appId>` |  
| Broken rolling restart of worker daemons | The rolling restart of the worker components for the HDFS, Yarn, HBase, Ambari Metrics and Spark2 services is broken, resulting in an HTTP 403 error. | All versions | For now, a workaround is to restart the respective service as a whole from service action menu by selecting `Restart All`.|
| Livy REST API | If you use the Livy REST API `GET /batches/{batchId}/log`, the logs are available only for a few hours after submitting the job. If you try to retrieve the logs many hours after submitting the job, the following error is displayed: `HTTP ERROR 500 : Problem accessing /gateway/default/livy/v1/batches. Reason: Server Error` | AE 1.1 | |
| Adding additional libraries | Livy or IBM Cloud CLI commands fail after installing additional packages, for example PyMySQL, manually on all nodes of a cluster because the package can't be found. | AE 1.1 clusters created before June 2019 | Navigate to **Ambari UI > Spark2 > Configs > Custom spark2-defaults > Add Property** and enter the following 2 lines:   \n    \n  `spark.yarn.appMasterEnv.PYSPARK3_PYTHON=/home/common/conda/anaconda3/bin/python`   \n    \n  `spark.yarn.appMasterEnv.PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python`   \n    \n  Restart the Spark service when prompted. This command forces the use of Anaconda Python instead of System Python. |
|  | If you install a newer version of a Python package than is already on the cluster and try to use it, the following error is displayed: `ImportError: No module named.` | AE 1.1 clusters created before June 2019 |Force the path to take the latest version by entering the following in your Python script:   \n  For Anaconda3: `import sys; sys.path.insert(0, '/home/wce/clsadmin/pipAnaconda3Packages')`   \n    \n  For Anaconda2: `import sys; sys.path.insert(0, '/home/wce/clsadmin/pipAnaconda2Packages')` |
| Disk space issues | HDFS audit logs are not rotated and so fill up the disk space, which disrupts the normal functioning of a cluster. | AE 1.2 clusters created before 10 September 2020 | Navigate to **Ambari UI > HDFS > Config > Advanced** and search in the filter for `hdfs-log4j`. At the bottom of the text box, add the following two lines that set the maximum log file size and the backup frequency to 30 days:   \n   \n `log4j.appender.DRFAAUDIT.MaxFileSize={{hadoop_log_max_backup_size}}MB`  \n   \n `log4j.appender.DRFAAUDIT.MaxBackupIndex={{hadoop_log_number_of_backup_files}}`|
