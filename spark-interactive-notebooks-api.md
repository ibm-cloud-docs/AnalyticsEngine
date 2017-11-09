---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Spark Interactive (Notebooks and API)

There are two ways to run spark applications interactively in an IBM Analytics Engine cluster:
* Jupyter Notebook Gateway (JNBG)
* SSH

## Jupyter Notebook Gateway (JNBG)

The IBM Analytics Engine cluster runs a JNBG service which is a Jupyter Kernel Gateway to allow interactive clients like Jupyter Notebook servers to connect to the cluster and submit code for execution.

### Supported Kernels

Currently, the JNBG service supports the following kernels:

* Python 2.7 with Spark 2.1
* Python 3.5 with Spark 2.1
* Scala 2.11 with Spark 2.1
* R with Spark 2.1

### SSH

You can run Spark applications interactively by logging to the cluster using SSH.

To run Spark applications interactively:

1. Log on to the cluster management node.
```
 $ ssh clsadmin@iae-tmp-867-mn003.bi.services.us-south.bluemix.net
```
2. You can start Python 2, Python 3, Scala and R interactive shells on the cluster:

  * Run Spark applications interactively with Python 2:
```
PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python pyspark \
     --master yarn \
     --deploy-mode client
 ```
 * Run Spark applications interactively with Python 3:
```
PYSPARK_PYTHON=/home/common/conda/anaconda3/bin/python pyspark \
     --master yarn \
     --deploy-mode client
 ```
 * Run Spark applications interactively with Scala:
``` spark-shell \
     --master yarn \
     --deploy-mode client
```
  * Run Spark applications interactively with R:
```
sparkR \
     --master yarn \
     --deploy-mode client
```

### Topic Areas
* [Accessing the JNBG service](./access-JNBG-service.html)
* [Monitor applications](./Monitor-Applications.html)
* [Kernel settings](./Kernel-Settings.html)
  * [Memory settings](./Kernel-Settings.html#memory-settings-for-kernels)
  * [Executor settings](./Kernel-Settings.html#executor-settings-for-kernel-applications)
  * [Number of concurrent kernels](./Kernel-Settings.html#number-of-concurrent-kernels)
  * [Interactive application name](./Kernel-Settings.html#interactive-application-name)
  * [Overriding kernel settings](./Kernel-Settings.html#overriding-kernel-settings---kernel_spark_args)
* [Lazy Spark initialization](./lazy-spark-initialization.html)
* [Logs](./Logs-JNBG.html)
  * [Jupyter Notebook Gateway server log](./Logs-JNBG.html#accessing-jupyter-kernel-gateway-log)
  * [Accessing kernel and driver logs](./Logs-JNBG.html#accessing-kerneldriver-logs)
  * [Accessing Spark executor logs](./Logs-JNBG.html#accessing-spark-executor-logs)
* [Installed libraries](./Installed-Libraries.html)
  * [Python](./Installed-Libraries.html#python)
  * [R](./Installed-Libraries.html#r)
  * [Scala/Java](./Installed-Libraries.html#scalajava)
* [Installing additional libraries](./installing-additional-libraries.html)
  * [Cluster wide installation](./installing-additional-libraries.html#cluster-wide-installation)
    * [Customization examples](./Customization-script-on-Bluemix-Object-Store.html)
  * [Notebook and session specific installation](./installing-additional-libraries.html#notebook--interactive-session-specific-installation)
  * [Local node installation](./installing-additional-libraries.html#node-local-installation)
* [Troubleshooting](./Troubleshooting-JKG.html)
* [Starting and stopping the JNBG service](./Stop,-Start,-Restart-JNBG-Service.html)


## Resources
* [Jupyter Kernel Gateway Reference Doc](https://jupyter-kernel-gateway.readthedocs.io/en/latest)
