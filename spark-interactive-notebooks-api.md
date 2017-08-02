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

# Spark Interactive (Notebooks and API)

## Jupyter Notebook Gateway (JNBG)

The IBM Analytics Engine cluster runs a JNBG service which is a Jupyter Kernel Gateway to allow interactive clients like Jupyter Notebook servers to connect to the cluster and submit code for execution. 

### Supported Kernels

Currently, the JNBG service supports the following kernels:

* Python 2.7 with Spark 2.1 
* Python 3.5 with Spark 2.1
* Scala 2.11 with Spark 2.1
* R with Spark 2.1 

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
* [Logs](./Logs--(JNBG).html)
  * [Jupyter Notebook Gateway server log](./Logs--(JNBG).html#accessing-jupyter-kernel-gateway-log)
  * [Accessing kernel and driver logs](./Logs--(JNBG).html#accessing-kerneldriver-logs)
  * [Accessing Spark executor logs](./Logs--(JNBG).html#accessing-spark-executor-logs)
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
