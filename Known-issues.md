---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-22"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Known issues
{: #known-issues}

List of known issues in the release:

| Category | Problem | AE version | Workaround |
|---------|---------|------------|------------|
| UI | The Cluster Management user interface does not function well in Internet Explorer 11 | All versions | The Management user interface functions well in Chrome, Safari and Firefox. Use these browsers to access the user interface. |
| Yarn Application UI link to logs is broken | When you click **Yarn >  Quick Links > Resource Manager UI** in the Ambari UI and select your application on the Applications tab, you will notice that the link to the application logs at the bottom right of the page is broken. | AE 1.2 | SSH to the cluster and run `yarn logs --applicationId <appId>`|
| Spark History server | The URL to drill down into the stages of a Spark job is broken due to a bug in the Knox re-write rules. | AE 1.1 | Remove `amp%3B` from the stages URL. For example, replace the following broken re-write URL (the example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`): `https://chs-xxx-yyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxxx_yyyy/stages/stage?amp%3Battempt=0&id=2` by this workaround URL: `https://chs-yyy-yyyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxx_yyyy/stages/stage?attempt=0&id=2` . |
|Streaming tab in Spark History Server UI | Streaming tab is missing in the Spark History Server UI. It is only visible on the live UI. This functions as designed. | All versions |  Submit the Spark job in client mode as a YARN application. Then go to **Yarn > Quick Links > Resource Manager UI** and search for your launched application. Scroll to the extreme right and click **ApplicationMaster** to see the **Streaming** tab. For more details, see [Streaming tab not available in the Spark UI](https://community.cloudera.com/t5/Support-Questions/HDP-2-6-Spark-2-1-Streaming-tab-not-available-in-the-Spark/m-p/230447){: external}. |   
| Broken rolling restart of worker daemons | The rolling restart of the slave components for the HDFS, Yarn, HBase, Ambari Metrics and Spark2 services is broken, resulting in an HTTP 403 error. | All versions | For now, a workaround is to restart the respective service as a whole from service action menu by selecting `Restart All`.|
| Livy REST API | If you use the Livy REST API `GET /batches/{batchId}/log`, the logs are available only for a few hours after submitting the job. If you try to retrieve the logs many hours after submitting the job, the following error is displayed: `HTTP ERROR 500 : Problem accessing /gateway/default/livy/v1/batches. Reason: Server Error` | AE 1.1 | |
| Adding additional libraries | Livy or IBM Cloud CLI commands fail after installing additional packages, for example PyMySQL, manually on all nodes of a cluster because the package can't be found. | AE1.1 clusters created before June 2019 | Navigate to **Ambari UI > Spark2 > Configs > Custom spark2-defaults > Add Property ** and enter the following 2 lines: <br> <br> `spark.yarn.appMasterEnv.PYSPARK3_PYTHON=/home/common/conda/anaconda3/bin/python` <br> <br> `spark.yarn.appMasterEnv.PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python` <br> <br> Restart the Spark service when prompted. This command forces the use of Anaconda Python instead of System Python. |
|  | If you install a newer version of a Python package than is already on the cluster and try to use it, the following error is displayed: `ImportError: No module named.` | AE1.1 clusters created before June 2019 |Force the path to take the latest version by entering the following in your Python script: <br> For Anaconda3: `import sys; sys.path.insert(0, '/home/wce/clsadmin/pipAnaconda3Packages')` <br> <br> For Anaconda2: `import sys; sys.path.insert(0, '/home/wce/clsadmin/pipAnaconda2Packages')` |
