---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-14"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Spark interactive (notebooks and API)
{: #spark-interactive}

There are two ways to run Spark applications interactively in an {{site.data.keyword.iae_full_notm}} cluster:

* By using the Jupyter Notebook Gateway (JNBG)
* By using SSH

## Jupyter Notebook Gateway (JNBG)

The {{site.data.keyword.iae_full_notm}} cluster runs a JNBG service which is a Jupyter Kernel Gateway to allow interactive clients like Jupyter notebook servers to connect to the cluster and submit code for execution.

### Spark kernels and libraries

See [Spark kernels and libraries on the cluster](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs) for a list of the libraries, which are pre-installed on each of the cluster nodes that are available by default on the kernels.

## SSH

You can run Spark applications interactively by logging on to the cluster using SSH.

To run Spark applications interactively:

1. Log on to the cluster management node.
```
$ ssh clsadmin@iae-tmp-867-mn003.<changeme>.ae.appdomain.cloud ```

  `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

2. You can start Python 2, Python 3, Scala, and R interactive shells on the cluster as follows:

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
 ```
 spark-shell \
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
* [Accessing the JNBG service](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-access-JNBG)
* [Monitor applications](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-monitoring-apps)
* [Kernel settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings)
  * [Memory settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#memory-settings-for-kernels)
  * [Executor settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#executor-settings-for-kernel-applications)
  * [Number of concurrent kernels](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#number-of-concurrent-kernels)
  * [Interactive application name](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#interactive-application-name)
  * [Overriding kernel settings](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#overriding-kernel-settings)
* [Lazy Spark initialization](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-lazy-spark-ini)
* [Logs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs)
  * [Jupyter Notebook Gateway server log](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-jupyter-kernel-gateway-logs)
  * [Accessing kernel and driver logs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-kernel-or-driver-logs)
  * [Accessing Spark executor logs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-spark-executor-logs)
* [Installed libraries](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs)
  * [Python](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#python)
  * [R](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#r)
  * [Scala or Java](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#scala-or-java)
* [Installing additional libraries](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs)
  * [Cluster wide installation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#cluster-wide-installation)
    * [Customization examples](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-cust-examples)
  * [Notebook and session specific installation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#notebook-or-interactive-session-specific-installations)
  * [Local node installation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#local-node-installation)
* [Troubleshooting](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-troubleshooting-JNBG)
* [Starting and stopping the JNBG service](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-start-stop-JNBG)


## Resources
* [Jupyter Kernel Gateway Reference Doc](https://jupyter-kernel-gateway.readthedocs.io/en/latest)
