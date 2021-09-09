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
{:external: target="_blank" .external}

# Spark interactive (notebooks and API)
{: #spark-interactive}

There are two ways to run Spark applications interactively in an {{site.data.keyword.iae_full_notm}} cluster:

- By using the [Jupyter Notebook Gateway (JNBG)](#jnbg-service)
- By using [SSH](#ssh-for-spark-interactive)

## Jupyter Notebook Gateway (JNBG)
{: #jnbg-service}

The {{site.data.keyword.iae_full_notm}} cluster runs a JNBG service which is a Jupyter Kernel Gateway to allow interactive clients like Jupyter notebook servers to connect to the cluster and submit code for execution.

### Spark kernels and libraries

See [Spark kernels and libraries on the cluster](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs){: new_window} for a list of the libraries, which are pre-installed on each of the cluster nodes that are available by default on the kernels.

## SSH
{: #ssh-for-spark-interactive}

You can run Spark applications interactively by logging on to the cluster using SSH.

To run Spark applications interactively:

1. Log on to the cluster management node.

    ```
    $ ssh clsadmin@iae-tmp-867-mn003.<changeme>.ae.appdomain.cloud
    ```

    Where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.
2. You can start Python 3, Scala, and R interactive shells on the cluster as follows:

  - Run Spark applications interactively with Python 3:

      ````
      PYSPARK_PYTHON=/home/common/conda/miniconda3.7/bin/python pyspark \
       --master yarn \
       --deploy-mode client
      ```
  - Run Spark applications interactively with Scala:

      ```
      spark-shell \
        --master yarn \
        --deploy-mode client
        ```
  - Run Spark applications interactively with R:

      ```
      sparkR \
          --master yarn \
          --deploy-mode client
      ```

### Topic Areas
* [Accessing the JNBG service](/docs/AnalyticsEngine?topic=AnalyticsEngine-access-JNBG){: new_window}
* [Monitor applications](/docs/AnalyticsEngine?topic=AnalyticsEngine-monitoring-apps){: new_window}
* [Kernel settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings){: new_window}
  * [Memory settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#memory-settings-for-kernels){: new_window}
  * [Executor settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#executor-settings-for-kernel-applications){: new_window}
  * [Number of concurrent kernels](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#number-of-concurrent-kernels){: new_window}
  * [Interactive application name](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#interactive-application-name){: new_window}
  * [Overriding kernel settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#overriding-kernel-settings){: new_window}
* [Lazy Spark initialization](/docs/AnalyticsEngine?topic=AnalyticsEngine-lazy-spark-ini){: new_window}
* [Logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs){: new_window}
  * [Jupyter Notebook Gateway server log](/docs/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-jupyter-kernel-gateway-logs){: new_window}
  * [Accessing kernel and driver logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-kernel-or-driver-logs){: new_window}
  * [Accessing Spark executor logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-JKG-logs#accessing-spark-executor-logs){: new_window}
* [Installed libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs){: new_window}
  * [Python](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#python){: new_window}
  * [R](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#r){: new_window}
  * [Scala or Java](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs#scala-or-java){: new_window}
* [Installing additional libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs){: new_window}
  * [Cluster wide installation](/docs/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#cluster-wide-installation){: new_window}
    * [Customization examples](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-examples){: new_window}
  * [Notebook and session specific installation](/docs/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#notebook-or-interactive-session-specific-installations){: new_window}
  * [Local node installation](/docs/AnalyticsEngine?topic=AnalyticsEngine-install-additional-libs#local-node-installation){: new_window}
* [Starting and stopping the JNBG service](/docs/AnalyticsEngine?topic=AnalyticsEngine-start-stop-JNBG){: new_window}


## Resources
* [Jupyter Kernel Gateway Reference Doc](https://jupyter-kernel-gateway.readthedocs.io/en/latest/){: external}
