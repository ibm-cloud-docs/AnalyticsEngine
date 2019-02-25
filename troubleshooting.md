---

copyright:
  years: 2017, 2019
lastupdated: "2018-10-08"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Troubleshooting
{: #Troubleshooting}

You can find the answers to common questions about how to use IBM Analytics Engine.

- [Jupyter Kernel Gateway](#jupyter-kernel-gateway)
- [Cluster management](#cluster-management)
- [Command line interface](#command-line-interface)
- [Log locations on cluster for various components](#log-locations-on-cluster-for-various-components)
- [Working with Hive](#working-with-hive)

## Jupyter Kernel Gateway

### When the notebook user interface opens, the kernel remains in a busy state (filled circle) although no code is running

 The notebook kernel is in busy state although no code cells are running because lazy evaluation could not be initialized on the Spark cluster and no more YARN containers are available on the cluster.

You can verify this by checking that the 'state' of your application is 'ACCEPTED' in the YARN RM UI.

To fix this issue, stop the existing running notebook kernels or any other YARN applications to free up resources.


## Cluster management

### When I open the cluster management page, an error message stating that I am not authorized appears

You might not be able to access the cluster management page for the following reasons:

1) You do not have developer access to the {{site.data.keyword.Bluemix_notm}} space.

2) Cookies are not enabled in your browser.

To fix this issue:

1) Work with the manager of your {{site.data.keyword.Bluemix_notm}} organization or space and get yourself developer privilege to the space containing the service instance you are attempting to access.

2) Ensure that cookies are enabled in your browser.

### No cluster is associated with my {{site.data.keyword.iae_full_notm}} service instance although I've waited for more than 30 minutes

No cluster was associated with your newly-created service instance although the service status shows that it was provisioned because the provisioning request didn't complete successfully.

You should delete your service instance and create a new one by using the {{site.data.keyword.Bluemix_notm}} catalog.

## Command line interface

### Enable tracing in the command line interface

Tracing can be enabled by setting `BLUEMIX_TRACE` environment variable to `true` (case ignored). When trace enabled additional debugging information will be printed on the terminal.

On Linux/macOS terminal

```
$ export BLUEMIX_TRACE=true
```

On Windows prompt

```
SET BLUEMIX_TRACE=true
```

To disable tracing set `BLUEMIX_TRACE` environment variable to `false` (case ignored)

### Endpoint was not set or found. Call endpoint first.

The Analytics Engine command-line interface requires a cluster endpoint to be first set. This enables the tool to talk to the cluster. The endpoint is the IP or hostname of the management node.

To set the cluster endpoint:

```
$ ibmcloud ae endpoint https://chs-nox-036-mn001.us-south.ae.appdomain.cloud
Registering endpoint 'https://chs-nox-036-mn001.us-south.ae.appdomain.cloud'...
Ambari Port Number [Optional: Press enter for default value] (9443)>
Knox Port Number [Optional: Press enter for default value] (8443)>
OK
Endpoint 'https://chs-nox-036-mn001.us-south.ae.appdomain.cloud' set.
```

## Log locations on cluster for various components

| Component | Node name | Log location |
|-----------|-----------|--------------|
|Ambari|mn003|`/var/log/ambari-server` </br> `/var/log/ambari-agent`|
|Apache Hadoop|mn003|History logs:`/var/log/hadoop-mapreduce/mapred/`</br>hadoop-mapreduce.jobsummary.log: `/var/log/hadoop-yarn/yarn`|
|Apache Livy|mn002|`/var/log/livy2`|
|Apache Phoenix|mn002|`/var/log/hbase/phoenix-hbase-server.log`|
|Apache Spark|mn003|`yarn logs –applicationID <appid>`</br>Spark History logs: `hadoop fs -ls /spark2-history`|
|Anaconda with Python|mn003|`/var/log/jnbg/kernel-log`|
|Flume|mn003|`/var/log/flume`|
|HBase|mn002|`/var/log/hbase`|
|Hive|mn003|`/var/log/hive2`|
|Jupyter Enterprise Gateway|mn003|Jupyter Kernel Gateway logs: `/var/log/jnbg/jupyter_kernel_gateway.log`</br>Kernel or driver logs: `/var/log/jnbg/kernel-.log`|
|Knox|mn002|`/var/log/knox/gateway.log`|
|Oozie|mn002|`/var/log/oozie`|
|Pig|mn003|`/var/log/ambari-server/tez-view`|
|Slider|mn003|`yarn logs -applicationId <applicationId>`|
|Sqoop|mn003|`/var/log/sqoop`|
|Tez|mn003|`/var/log/ambari-server/tez-view`|
|Yarn|mn003|`yarn logs -applicationId <applicationId>`|

## Working with Hive

###  Hive does not recognize Cloud Object Storage with HMAC style authentication

When Hive is run with Cloud Object Storage with HMAC style authentication, changes to the Cloud Object Storage configuration in the core-site.xml file are not picked up by Hive. The reason is that Hive does not recognize these changes unless Hive is restarted which does not happen by default.

To fix this issue, restart Hive.
