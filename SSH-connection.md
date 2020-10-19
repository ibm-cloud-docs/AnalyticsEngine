---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-19"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# SSH connection
{: #ssh-connection}

You can run spark-submit jobs by logging on to the cluster  using the SSH protocol.


1. Log on to the cluster management node using [SSH](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH). For example:
```
$ ssh clsadmin@iae-tmp-867-mn003.us-south.ae.appdomain.cloud
```

2. Run spark-submit. For example:
  ```
  $ spark-submit \
  --master yarn \
  --deploy-mode cluster \
  --class org.apache.spark.examples.SparkPi \
  /usr/hdp/current/spark2-client/jars/spark-examples.jar
  ```

## Running spark-submit with Anaconda Python 3

To run sprk-submit with Anaconda Python 3, enter:

  ```
  PYSPARK_PYTHON=/home/common/conda/miniconda3.7/bin/python spark-submit \
  --master yarn \
  --deploy-mode cluster  \
  /usr/hdp/current/spark2-client/examples/src/main/python/pi.py
  ```

For more information, see [Submitting applications](http://spark.apache.org/docs/latest/submitting-applications.html){: external}.
