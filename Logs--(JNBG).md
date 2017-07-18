---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Logs (JNBG)

## Accessing Jupyter Kernel Gateway log

The Jupyter Notebook Gateway Service log can be accessed on the [JNBG service host](./JNBG-Service-Host.html) at `/var/log/jnbg/jupyter-kernelgateway.log`. 

You can SSH to the [JNBG service host](./JNBG-Service-Host.html) and access the log at `/var/log/jnbg/jupyter-kernelgateway.log`.

## Accessing Kernel/Driver logs

All kernel logs are written to the `/var/log/jnbg` directory on the [JNBG service host](./JNBG-Service-Host.html). The kernel log files follow a naming convention of `kernel-<kernel-type>-<appname>-<YYYYMMDD_hhmmss>.log`. Here, 

* `<kernel-type>` is: `scala-spark21` for Scala with Spark 2.1, `python2-spark21` for Python 2.7 with Spark 2.1 kernel, `python3-spark21` for Python 3.5 with Spark 2.1 kernel and `r` for R with Spark 2.1 kernel.
* `<appname>` is the value of the `KERNEL_NAME` parameter used when creating the kernel, as described [above](#kernel-name). 
* `<YYYYMMDD_hhmmss>` is the kernel creation timestamp

Using the combination of `kernel-type`, `appname` and the `<YYYYMMDD_hhmmss>` timestamp, a user can easily locate the kernel log file corresponding to a particular session.

You can access the logs by SSHing to the [JNBG service host](./JNBG-Service-Host.html)  and access the kernel log file in the `/var/log/jnbg` directory, or access the log file contents from within a notebook/interactive session by executing 
  `cat /var/log/jnbg/<kernel-log-filename>` system command.
 
## Accessing Spark Executor logs

You can access Spark Executor logs using one of the following ways:

* launch the cluster's Ambari console to navigate to the corresponding YARN/Spark application UI and use the links in the web interface to download the logs. 

* a command line and a REST API interface are available to download the logs. Refer to the instructions [here](./wce-cli-ref-spark-logs.html) for details on these ways to download Spark Executor logs.

* SSH to the cluster and use the YARN command line interface to obtain the logs. To obtain logs for a particular `application Id` run the following command:

 ```
 yarn logs -applicationId <application Id>
 ```
 This will output to the console all the container logs associated with the `application Id`. Typically these would be the stderr and stdout logs for the Application Master and each Executor launched for the application session.

 Note that you can obtain the application ID for your notebook/interactive session by executing the following 
 code valid for both Python and Scala:

 ```
 sc.applicationId
 ```
