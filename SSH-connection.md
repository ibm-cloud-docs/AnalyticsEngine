---

copyright:
  years: 2017,2018
lastupdated: "2018-09-26"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# SSH connection

You can run spark-submit jobs by logging on to the cluster  using the SSH protocol.


1. Log on to the cluster management node using [SSH](/docs/services/AnalyticsEngine/Connect-using-SSH.html). For example:
```
$ ssh clsadmin@iae-tmp-867-mn003.us-south.ae.appdomain.cloud```

2. Run spark-submit. For example:
  ```
  $ spark-submit \
  --master yarn \
  --deploy-mode cluster \
  --class org.apache.spark.examples.SparkPi \
  /usr/hdp/current/spark2-client/jars/spark-examples.jar
  ```

**To run spark-submit with Anaconda Python 2**

  ```
  PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python spark-submit \
  --master yarn \
  --deploy-mode cluster  \
  /usr/hdp/current/spark2-client/examples/src/main/python/pi.py
  ```

**To run spark-submit with Anaconda Python 3**

  ```
  PYSPARK_PYTHON=/home/common/conda/anaconda3/bin/python spark-submit \
  --master yarn \
  --deploy-mode cluster  \
  /usr/hdp/current/spark2-client/examples/src/main/python/pi.py
```

For more information, see [Submitting applications](http://spark.apache.org/docs/latest/submitting-applications.html).
