---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-12"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:support: data-reuse='support'}

# Troubleshooting
{: #troubleshooting}

In this topic, you can find the answers to common questions about how to use {{site.data.keyword.iae_full_notm}}.

## Jupyter Kernel Gateway: Notebook kernel in busy state although no code is running
{: #kernel-state-busy}
{: troubleshoot}
{: support}

You might see messages like `Waiting for a Spark session to start...` or `Obtaining Spark session...` when you execute code in a notebook and notice that no code is running although the notebook kernel is in busy state. The reason is that lazy evaluation could not be initialized on the Spark cluster because no more YARN containers are available on the cluster.

You can verify this by checking that the 'state' of your application is 'ACCEPTED' in the YARN Resource Manager UI.

To free YARN resources, stop other running notebook kernels or interactive sessions, and any YARN applications.

## Cluster management: No access to cluster management page
{: #no-access-cluster-management}

When you open the cluster management page, you might see an error message stating that you aren't authorized to access the page.

You might not be able to access the cluster management page for the following reasons:

1. You do not have developer access to the {{site.data.keyword.Bluemix_notm}} space.

2. Cookies are not enabled in your browser.

To fix this issue:

1. Work with the manager of your {{site.data.keyword.Bluemix_notm}} organization or space and get yourself developer privilege to the space containing the service instance you are attempting to access.

2. Ensure that cookies are enabled in your browser.

## Cluster management: No cluster for my service instance
{: #no-cluster-for-instance}
{: troubleshoot}
{: support}

No cluster is associated with my {{site.data.keyword.iae_full_notm}} service instance although I've waited for more than 50 minutes

No cluster was associated with your newly-created service instance although the service status shows that it was provisioned because the provisioning request didn't complete successfully.

You should delete your service instance and create a new one by using the {{site.data.keyword.Bluemix_notm}} catalog.

## Command line interface: Enable tracing in the command line interface
{: #tracing-in-cli}
{: troubleshoot}
{: support}

Tracing can be enabled by setting `BLUEMIX_TRACE` environment variable to `true` (case ignored). When trace is enabled,  additional debugging information is printed to the terminal.

On Linux/macOS terminal:

```
$ export BLUEMIX_TRACE=true
```

On Windows prompt:

```
SET BLUEMIX_TRACE=true
```

To disable tracing, set the `BLUEMIX_TRACE` environment variable to `false` (case ignored).

## Command line interface: No cluster endpoint found
{: #no-cluster-endpoint}
{: troubleshoot}
{: support}

The {{site.data.keyword.iae_full_notm}} command-line interface requires a cluster endpoint to be first set. This enables the tool to talk to the cluster. The endpoint is the IP or hostname of the management node.

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
{: #log-locations-on-cluster}
{: troubleshoot}
{: support}

The following tables list the location of the log files for different components on the cluster.

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


## Working with Hive: Changes to the Cloud Object Storage configuration not detected
{: #hive-cos-troubleshooting}
{: troubleshoot}
{: support}

When Hive is run with {{site.data.keyword.cos_full_notm}} with HMAC style authentication, changes to the {{site.data.keyword.cos_short}} configuration in the `core-site.xml` file are not picked up by Hive. The reason is that Hive does not recognize these changes unless Hive is restarted which does not happen by default.

To fix this issue, restart Hive.

## Working with Hive: Hive metastore can't be started on newly created cluster

If you create new {{site.data.keyword.iae_full_notm}} clusters and you associate an existing IBM Cloud Databases for PostgreSQL instance created before 12 May 2020 with your clusters, you will see the following error message:

```
ERROR :relation "BUCKETING_COLS" already exists
```

The reason is a schema version change. To continue using the same database instance, you must run the following command on the `mn002` node of the cluster:

```
/usr/hdp/current/hive-server2/bin/schematool -url 'jdbc:postgresql://<YOUR-POSTGRES-INSTANCE-HOSTNAME>:<PORT>/ibmclouddb?sslmode=verify-ca&sslrootcert=<PATH/TO/POSTGRES/CERT>' -dbType postgres -userName <USERNAME> -passWord <PASSWORD> -upgradeSchema 3.1.1000 -verbose
```

This error does not occur if you associate a new IBM Cloud Databases for PostgreSQL instance with the {{site.data.keyword.iae_full_notm}} clusters.
