---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-17"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Jupyter Kernel Gateway Logs

## Accessing Jupyter Kernel Gateway logs

The Jupyter Notebook Gateway Service log can be accessed on the [Jupyter Kernel Gateway (JNBG) service host](/docs/services/AnalyticsEngine/JNBG-Service-Host.html) at `/var/log/jnbg/jupyter_kernelgateway.log`.

You can SSH to the [JNBG service host](/docs/services/AnalyticsEngine/JNBG-Service-Host.html) and access the log at `/var/log/jnbg/jupyter_kernelgateway.log`.

## Accessing Kernel or Driver logs

All kernel logs are written to the `/var/log/jnbg` directory on the [JNBG service host](/docs/services/AnalyticsEngine/JNBG-Service-Host.html). The kernel log files use the following naming convention:
```
kernel-<kernel-type>-<appname>-<YYYYMMDD_hhmmss>.log ```
where:
* `<kernel-type>` is: `scala-spark21` for Scala with Spark 2.1, `python2-spark21` for Python 2.7 with a Spark 2.1 kernel, `python3-spark21` for Python 3.5 with a Spark 2.1 kernel and `r` for R with a Spark 2.1 kernel.
* `<appname>` is the value of the `KERNEL_NAME` parameter used when creating the kernel.
* `<YYYYMMDD_hhmmss>` is the kernel creation timestamp.

Using the combination of `kernel-type`, `appname` and the `<YYYYMMDD_hhmmss>` timestamp, you can easily locate the kernel log file corresponding to a particular session.

You can access the logs by SSHing to the [JNBG service host](/docs/services/AnalyticsEngine/JNBG-Service-Host.html)  and accessing the kernel log file in the `/var/log/jnbg` directory. Alternatively, you can access the log file contents from within a notebook or interactive session by executing the following system command:
```
cat /var/log/jnbg/<kernel-log-filename>```

## Accessing Spark Executor logs

You can access Spark Executor logs using one of the following ways:

* Launching the cluster's Ambari console to navigate to the corresponding YARN/Spark application UI and use the links in the web interface to download the logs.

* Using a command line and REST API interface to download the logs. Refer to the instructions [here](/docs/analytics-engine-cli-plugin/analytics-engine-service-cli.html#spark‐logs) for details on how to download Spark Executor logs.

* SSHing to the cluster and using the YARN command line interface to obtain the logs. To obtain logs for a particular `application Id` run the following command:

 ```
 yarn logs -applicationId <application Id>
 ```
 {:codeblock}

 This will output to the console all the container logs associated with the `application Id`. Typically these would be the stderr and stdout logs for the Application Master and each Executor launched for the application session.

 Note that you can obtain the application ID for your notebook or interactive session by executing the following
 code valid for both Python and Scala:

 ```
 sc.applicationId
 ```
 {: codeblock}
