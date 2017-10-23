---

copyright:
  years: 2017
lastupdated: "2017-10-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Kernel settings

## Memory settings for kernels

### Default
  No explicit memory limit set on a per Kernel basis. 6 GB free memory available on the container where kernels are run.

### How to change  
  Refer to [overriding kernel settings section](./Kernel-Settings.html#overriding-kernel-settings---kernel_spark_args) to see how you can pass settings to control driver and executor memory limits. By default there is no limit applied to the driver process and the Spark default is used for executors (~ 1 GB). Note that for Python and R kernels, the Python and R native process can consume it's own memory which may not be governed by the `--driver-memory` option. There is no memory limit applied to these processes.

## Executor settings for kernel applications  

### Defaults

  Number of executors: 2  

  Number of cores per executor: 1  

  Memory per executor: 1.5 GB (1 GB Spark default + 10% YARN overhead memory - see `spark.yarn.executor.memoryOverhead` [here](https://spark.apache.org/docs/latest/running-on-yarn.html))

 ### How to change
To change settings globally for the cluster, use the Ambari console and update the corresponding property parameters. A number of settings may be updated, including the following:

Custom spark2-defaults:
* No of Executors per Application - Property: spark.executor.instances="2"  

Advanced spark2-env:
* Number of Cores per Executor - Property: SPARK_EXECUTOR_CORES="1"  
* Memory per Executor - Property: SPARK_EXECUTOR_MEMORY="1G"  

Then save and restart all the affected components.

To change settings per notebook and kernel, refer to the [overriding kernel settings section](./Kernel-Settings.html#overriding-kernel-settings---kernel_spark_args).

## Number of concurrent kernels
  There is no explicit limit set on numbers of kernels.The max number of kernel depends on the containers available in cluster.  

  On a 1 Data Node Cluster with above defaults  
   Max concurrent kernels - 3

  On a 2 Data Node Cluster with above defaults  
   Max concurrent kernels - 7

  _The Math_ :

A Data node has 4 vcores and 14 GB Memory.

As per default settings above, a kernel app will use 1 container for ApplicationMaster and 2 containers for executors (1 container per executor).

* The ApplicationMaster container uses 1 GB memory (512 MB + 10% YARN overhead ~ 1GB YARN container)
* Each executor container uses 1.5 GB memory  

Therefore, with these defaults, a Kernel will require 1 + 2 * 1.5 = 4 GB.

## Interactive application name

The IBM Analytics Engine cluster will typically be used to submit code for execution on Spark. Every notebook or interactive client will launch a long running Spark session to execute Spark code. Often a user will want to monitor the Spark jobs, executors or kernel logs related to the interactive session. The usual method of retrieving the Spark application ID and using it to look up the corresponding entries in YARN or Spark History Server UI etc is not very user friendly on it's own.

To make this easier, the JKG service and all the kernels on the IBM Analytics Engine cluster are configured to accept a `KERNEL_APPNAME` parameter from interactive clients. When this parameter is provided, its value is used to set the YARN/Spark application name associated with the session. Similarly, the kernel log files generated for the session include the `KERNEL_APPNAME` value in the file name.

It is expected that all interactive clients specify the `KERNEL_APPNAME` parameter. Notebook servers are expected to pass the notebook name as the value of the `KERNEL_APPNAME` parameter.

To specify the `KERNEL_APPNAME`, the interactive client should include it within an `env` json in the `POST` method body when invoking the Kernel Gateway API to start a kernel:

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

A sample POST request would look so:

```
POST /gateway/default/jkg/api/kernels HTTP/1.1
Host: chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443
Authorization: Basic d2NlYWRtaW46NWF1dUY1U1UzZTBH
Connection: close
Accept-Encoding: gzip,deflate
Content-Type: application/json
Content-Length: 63

{"name": "r-spark21", "env": {"KERNEL_APPNAME": "ExampleName"}}
```

Note that currently the Apache Toree - Scala kernel will accept the `KERNEL_APPNAME` parameter and include it in the kernel log file name, but the YARN/Spark application name will show as `Apache Toree`. This issue will be fixed in future ensuring the application name matches the specified `KERNEL_APPNAME`.


## Overriding Kernel Settings - KERNEL_SPARK_ARGS

The desired kernel settings can be specified when requesting a new kernel from the Jupyter Notebook Gateway service. To do this, include a `KERNEL_SPARK_ARGS` key in the `env` object of the kernel request POST body. The value of the `KERNEL_SPARK_ARGS` can be a string containing any of the valid Spark options that can generally be specified on the Spark command line when launching a Spark application.

So here are some examples of how you can control Kernel settings:

| KERNEL_SPARK_ARGS  | Request JSON             | Remarks |
|--------------------|--------------------------|----------|
| `--num-executors 3` | {"name": "r-spark21", "env": {"KERNEL_SPARK_ARGS": "--num-executors 3"}} | Start a "r-spark21" kernel and acquire 3 executors for it |
| `--conf spark.executor.memory=2G` | {"name": "python2-spark21", "env": {"KERNEL_SPARK_ARGS": "--conf spark.executor.memory=2G"}} | Start a "python2-spark21" kernel for where each executor can use up to 2GB memory |
| `--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G` | {"name": "scala-spark21", "env": {"KERNEL_SPARK_ARGS": "--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G"}} | Start a "scala-spark21" kernel where the driver process can take upto 2 GB memory, 4 executors are acquired and each executor can use up to 2GB memory|

Here's an example HTTP request showing the use of `KERNEL_SPARK_ARGS` when requesting a new kernel:

```
POST /gateway/default/jkg/api/kernels HTTP/1.1
Host: iaehost:8443
Authorization: Basic Y2xzYWRtaW46MXFhenhzdzIzZWRj
Accept: */*
Content-Type: application/json
Content-Length: 131


{"name": "spark_2.1_python2", "env": {"KERNEL_SPARK_ARGS": "--driver-memory 2G --num-executors 4 --conf spark.executor.memory=2G"}}
```
When kernel settings are not specified this way, the respective Spark defaults currently configured on your cluster take effect.
