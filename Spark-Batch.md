---

copyright:
  years: 2017
lastupdated: "2017-09-19"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Spark batch jobs

There are three ways to submit a Spark batch job to an IBM Analytics Engine cluster.

1. [IBM Analytics Engine Command Line Interface (CLI)](#IBM-Analytics-Engine-cli) (recommended)
2. [Livy API](./Livy-api.html)
3. [SSH](#ssh)

The instructions below provide a simple example of submitting a Spark batch application using each mechanism.

---

## IBM Analytics Engine CLI

The easiest way to submit and manage a Spark batch job is using the [IBM Analytics Engine CLI](./WCE-CLI.html).

To submit a spark batch job

1. [Download and Install](./wce-wcl-install.html) the CLI plugin.
2. Run [spark-endpoint](./wce-cli-ref-spark-endpoint.html) command to set the cluster endpoint.

  The argument for `endpoint` is the ip or hostname of cluster management node

  ```
  $ bx ae endpoint https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net
  ```

  Hit return on prompts to accept the default port numbers for Ambari(9443) and Knox(8443) services.

  Response:

  ```
  Registering endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net'...
  Ambari Port Number [Optional: Press enter for default value] (9443)>
  Knox Port Number [Optional: Press enter for default value] (8443)>
  OK
  Endpoint 'https://ae-tmp-867-mn001.bi.services.us-south.bluemix.net' set.
  ```

3. Run [spark-submit](./wce-cli-ref-spark-submit.html) command.

  ```
  $ bx ae spark-submit --className org.apache.spark.examples.SparkPi local:/usr/hdp/current/spark2-client/jars/spark-examples.jar
  ```

  Enter the IBM Analytics Engine cluster login credentials at the prompts. To set the default username for command execution see [`username`](./wce-cli-ref-username.html) command.

  Response:

  ```
  User (clsadmin)>
  Password>
  Contacting endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net:8443'...
  Job ID '17'
  Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
  Finished contacting endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net:8443'
  OK
  Job ID '17'
  Application ID 'application_1491850285904_0023'
  Done
  ```

For more information see the following:
  * [IBM Analytics Engine CLI](./WCE-CLI.md)
  * [IBM Analytics Engine CLI - spark-submit](./wce-cli-ref-spark-submit.md)

---

## SSH

You can run spark-submit by logging to the cluster using [SSH](./Connect-using-SSH.html).

***To log on to the cluster using SSH***

1. Log on to the cluster management node.

  ```
  $ ssh clsadmin@iae-tmp-867-mn003.bi.services.us-south.bluemix.net
  ```

2. Run spark-submit.

  ```
  $ spark-submit \
  --master yarn \
  --deploy-mode cluster \
  --class org.apache.spark.examples.SparkPi \
  /usr/hdp/current/spark2-client/jars/spark-examples.jar
  ```

***To run spark-submit with Anaconda Python 2***

  ```
  PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python spark-submit \
  --master yarn \
  --deploy-mode cluster  \
  /usr/hdp/current/spark2-client/examples/src/main/python/pi.py
  ```

***To run spark-submit with Anaconda Python 3***
  ```
  PYSPARK_PYTHON=/home/common/conda/anaconda3/bin/python spark-submit \
  --master yarn \
  --deploy-mode cluster  \
  /usr/hdp/current/spark2-client/examples/src/main/python/pi.py```
```

For more information see the following:
  * [Connect using SSH](./Connect-using-SSH.html).
  * [Apache Spark Submitting Applications](http://spark.apache.org/docs/latest/submitting-applications.html).
