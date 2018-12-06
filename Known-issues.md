---

copyright:
  years: 2017,2018
lastupdated: "2018-09-24"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Known issues

List of known issues in the release:

| Category | Problem | Workaround |
|------------|-----------|-----------|
| Cluster Access | Launch of Ambari console or SSH connectivity to newly created cluster may fail due to a SoftLayer DNS issue | Allow a few minutes prior to access.|
| UI | The Cluster Management user interface does not function well in Internet Explorer 11 | The Management user interface functions fine in Chrome, Safari and Firefox. Use these browsers to access the user interface. |
| Browser | {{site.data.keyword.iae_full_notm}} service instance creation and management can only be done on Firefox version 60 or earlier. There is a known issue about supporting later versions of Firefox. | Either use Firefox version 60 or earlier, or switch to another supported browser like Chrome or Safari. |
| Oozie | Oozie jobs fail because of the Oozie versions used with HDP. | Perform the steps in the following [workaround](./workaround-oozie-jobs.html). |
| Customization | The package-admin can only install packages in the centos repository. | |
| | OS packages that are installed through the package-admin will not persist after the host is rebooted. They need to be installed again, if the host machine is rebooted. | |   
| Spark History server | The URL to drill down into the stages of a Spark job is broken due to a bug in the Knox re-write rules. | Remove &`amp%3B` from the stages URL. For example, replace the following broken re-write URL (the example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`): `https://chs-xxx-yyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxxx_yyyy/stages/stage?amp%3Battempt=0&id=2` by this workaround URL: `https://chs-yyy-yyyy-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxx_yyyy/stages/stage?attempt=0&id=2` . |
| | Spark History server doesn't show applications on clusters with Spark 2.3. | Navigate to **Ambari UI > Spark2 > Configs > Advanced spark2-env > content** and add the following line: </br> `export SPARK_HISTORY_OPTS="-Dspark.ui.proxyBase=/gateway/default/sparkhistory" ` </br> Save your changes and restart your cluster. |
|Streaming tab in Spark History Server UI | Streaming tab is missing in the Spark History Server UI. It is only visible on the live UI. This functions as designed. | Submit the Spark job in client mode as a YARN application. Then go to **Yarn > Quick Links > Resource Manager UI** and search for your launched application. Scroll to the extreme right and click **ApplicationMaster** to see the **Streaming** tab. For more details, see https://community.hortonworks.com/questions/110212/hdp-26-spark-21-streaming-tab-not-available-in-the.html. |   
| Broken rolling restart of worker daemons | The rolling restart of the slave components for the HDFS, Yarn, HBase, Ambari Metrics and Spark2 services is broken, resulting in an HTTP 403 error.  | For now, a workaround is to restart the respective service as a whole from service action menu by selecting `Restart All`.|
