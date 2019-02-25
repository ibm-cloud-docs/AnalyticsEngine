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


# Kernel settings
{: #kernel-settings}

## Memory settings for kernels

No explicit memory limit is set on a per kernel basis. For clusters running on the `Default` hardware configuration, 6 GB of free memory is available to run the kernels. For clusters running on the `Memory Intensive` hardware configuration, 96 GB of free memory is available to run the kernels.

### Changing memory settings  

Refer to the [overriding kernel settings section](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#overriding-kernel-settings) to see how you can pass settings to control driver and executor memory limits. By default, no limit is applied to the driver process and the Spark default is used for executors (approximately 1 GB).

Note that for Python and R kernels, the Python and R native process can consume it's own memory which may not be governed by the `--driver-memory` option. There is no memory limit applied to these processes.

## Executor settings for kernel applications  

The following executor settings are the defaults:

-	Number of executors: Spark dynamic allocation is enabled (the property `spark.dynamicAllocation.enabled` is set to true), with the lower bound (designated by the property `spark.dynamicAllocation.minExecutors`) set to 1 executor. No upper bound is set on the maximum number of executors (designated by the property `spark.dynamicAllocation.maxExecutors` which is set to infinity).                                     
**Important:** As no upper bound is set, certain kernels or Spark applications could utilize the entire cluster, leaving no resources for other kernels or Spark applications to start.
- Number of cores per executor: 1  
- Memory available per executor: 1.5 GB (1 GB Spark default + 10% YARN overhead memory. For details, see  [`spark.yarn.executor.memoryOverhead`](https://spark.apache.org/docs/latest/running-on-yarn.html).

### Changing executor settings

With Spark dynamic allocation enabled by default, your kernel or Spark application to fully utilize the cluster. If your applications have specific requirements and you want to have control over the number of executors per application, you can achieve that by updating the corresponding property parameters through the Ambari console or by [providing custom configurations](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options). You can update a number of settings, including the following:

Custom spark2-defaults:
*	`spark.dynamicAllocation.enabled="false"`: Disables Spark dynamic allocations.  
* `spark.executor.instances`: The number of executors per application. For example, `spark.executor.instances="2"`  

Advanced spark2-env:
* `SPARK_EXECUTOR_CORES`: The number of cores per executor. For example, `SPARK_EXECUTOR_CORES="1"`  
* `SPARK_EXECUTOR_MEMORY`: Memory per executor. For example, `SPARK_EXECUTOR_MEMORY="1G"`  

Save your changes and restart all the affected components.

To change settings per notebook and kernel, refer to the [overriding kernel settings section](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#overriding-kernel-settings).

## Number of concurrent kernels
There is no explicit limit set on numbers of kernels. The maximum number of kernels depends on the nodes available in the cluster.

For example:  

- On a 1 data node cluster with default settings, the maximum concurrent kernels is 3.

- On a 2 data node cluster with default settings, the maximum  concurrent kernels is 7.

_The math_ :

A data node has 4 vCores and 14 GB memory.

If you use the default settings, a kernel app will use 1 container for the ApplicationMaster and 2 containers for the executors (1 container per executor).

* The ApplicationMaster container uses 1 GB memory (512 MB + 10% YARN overhead ~ 1 GB YARN container)
* Each executor container uses 1.5 GB memory  

Therefore, using the defaults, a kernel will require 1 + 2 * 1.5 = 4 GB.

## Interactive application name

The {{site.data.keyword.iae_full_notm}} cluster will typically be used to submit code for execution on Spark. Every notebook or interactive client will launch a long running Spark session to execute Spark code. Often a user will want to monitor the Spark jobs, executors or kernel logs related to the interactive session. The usual method of retrieving the Spark application ID and using it to look up the corresponding entries in YARN or the Spark History Server UI is not very user friendly on it's own.

To make this easier, the JKG service and all the kernels on the {{site.data.keyword.iae_full_notm}} cluster are configured to accept a `KERNEL_APPNAME` parameter from interactive clients. When this parameter is provided, its value is used to set the YARN/Spark application name associated with the session. Similarly, the kernel log files generated for the session include the `KERNEL_APPNAME` value in the file name.

It is expected that all interactive clients specify the `KERNEL_APPNAME` parameter. Notebook servers are expected to pass the notebook name as the value of the `KERNEL_APPNAME` parameter.

To specify the `KERNEL_APPNAME`, the interactive client should include it in an `env` JSON in the `POST` method body when invoking the Kernel Gateway API to start a kernel. For example:

```

URL : <JKG Endpoint>/api/kernels
METHOD: POST
BODY:
{
"name": "<KERNELSPEC_NAME>",
 "env": {
            "KERNEL_APPNAME": "<NOTEBOOK_NAME>"
        }
}
```

A sample POST request:

```
POST /gateway/default/jkg/api/kernels HTTP/1.1
Host: chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443
Authorization: Basic d2NlYWRtaW46NWF1dUY1U1UzZTBH
Connection: close
Accept-Encoding: gzip,deflate
Content-Type: application/json
Content-Length: 63

{"name": "r-spark21", "env": {"KERNEL_APPNAME": "ExampleName"}}
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

Note that currently the Apache Toree - Scala kernel will accept the `KERNEL_APPNAME` parameter and include it in the kernel log file name, but the YARN/Spark application name will show as `Apache Toree`. This issue will be fixed in future ensuring that the application name matches the specified `KERNEL_APPNAME`.


## Overriding kernel settings

The desired kernel settings can be specified when requesting a new kernel from the Jupyter Notebook Gateway service. To do this, include a `KERNEL_SPARK_ARGS` key in the `env` object of the kernel request POST body. The value of the `KERNEL_SPARK_ARGS` can be a string containing any of the valid Spark options that can generally be specified on the Spark command line when launching a Spark application.

So here are some examples of how you can control kernel settings:

| KERNEL_SPARK_ARGS  | Request JSON             | Remarks |
|--------------------|--------------------------|----------|
| `--num-executors 3` | {"name": "r-spark21", "env": {"KERNEL_SPARK_ARGS": "--num-executors 3"}} | Starts a "r-spark21" kernel and acquires 3 executors for it |
| `--conf spark.executor.memory=2G` | {"name": "python2-spark21", "env": {"KERNEL_SPARK_ARGS": "--conf spark.executor.memory=2G"}} | Starts a "python2-spark21" kernel where each executor can use up to 2 GB memory |
| `--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G` | {"name": "scala-spark21", "env": {"KERNEL_SPARK_ARGS": "--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G"}} | Starts a "scala-spark21" kernel where the driver process can take up to 2 GB memory, 4 executors are acquired and each executor can use up to 2 GB memory|

The following example shows an HTTP request using `KERNEL_SPARK_ARGS` to request a new kernel:

```
POST /gateway/default/jkg/api/kernels HTTP/1.1
Host: iaehost:8443
Authorization: Basic Y2xzYWRtaW46MXFhenhzdzIzZWRj
Accept: */*
Content-Type: application/json
Content-Length: 131


{"name": "spark_2.1_python2", "env": {"KERNEL_SPARK_ARGS": "--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G"}}
```
When the kernel settings are not specified in this way, the respective Spark defaults currently configured on your cluster take effect.
