---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

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
| Cluster Access | Launch of Ambari Console or SSH connectivity to newly created cluster may fail due to a SoftLayer DNS issue | Allow a few minutes prior to access.|
| UI | The Cluster Management user interface does not function well in Internet Explorer 11 | The Management user interface functions fine in Chrome, Safari and Firefox. Use these browsers to access the user interface. |
| Oozie | Oozie jobs fail because of the Oozie versions used with HDP. | Perform the steps in the following [workaround](./workaround-oozie-jobs.html). |
| Customization | The package-admin can only install packages in the centos repository. | |
| | OS packages that are installed through the package-admin will not persist after the host is rebooted. They need to be installed again, if the host machine is rebooted. | |   
| Spark History server | The URL to drill down into the stages of a Spark job is broken due to a bug in the Knox re-write rules. | Remove &`amp%3B` from the stages URL. For example, replace the following broken re-write URL: `https://chs-xxx-yyy-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxxx_yyyy/stages/stage?amp%3Battempt=0&id=2` by this workaround URL: `https://chs-yyy-yyyy-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/sparkhistory/history/application_xxxxxxxxxx_yyyy/stages/stage?attempt=0&id=2`  |
| Broken rolling restart of worker daemons | The rolling restart of the slave components for the HDFS, Yarn, HBase, Ambari Metrics and Spark2 services is broken, resulting in an HTTP 403 error.  | For now, a workaround is to restart the respective service as a whole from service action menu by selecting `Restart All`.|
