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

# Spark batch jobs

There are three ways to submit a Spark batch job to an IBM Analytics Engine cluster.

1. [IBM Analytics Engine Command Line Interface (CLI)](#IBM-Analytics-Engine-cli) (recommended)
2. [Livy API](#livy-api)
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
  $ bx iae endpoint https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net
  ```

  Hit return on prompts to accept the default port numbers for Ambari(9443) and Knox(8443) services.

  Response:

  ```
  Registering endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net'...
  Ambari Port Number [Optional: Press enter for default value] (9443)>
  Knox Port Number [Optional: Press enter for default value] (8443)>
  OK
  Endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net' set.
  ```

3. Run [spark-submit](./wce-cli-ref-spark-submit.html) command.

  ```
  $ bx iae spark-submit --className org.apache.spark.examples.SparkPi local:/usr/iop/current/spark2-client/jars/spark-examples.jar
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

## Livy API

[Livy](https://github.com/cloudera/livy) is an open source REST interface for submitting batch jobs to Apache Spark on an IBM Analytics Engine cluster.

The Livy API is routed through Apache Knox, so all URL's should be modified to include prefix `/gateway/default/livy/v1` before the API URL. For example, `/batches` becomes `/gateway/default/livy/v1/batches`.

The following example commands are piped through [jq](https://stedolan.github.io/jq/) to pretty print the JSON output.

To submit a spark batch using the Livy API

```bash
curl -k \
-u "<user>:<password>" \
-H 'Content-Type: application/json' \
-d '{ "file":"local:/usr/iop/current/spark2-client/jars/spark-examples.jar", "className":"org.apache.spark.examples.SparkPi" }' \
"https://wce-tmp-867-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
```

Successful response:

```json
{
  "id": 21,
  "state": "starting",
  "appId": null,
  "appInfo": {
    "driverLogUrl": null,
    "sparkUiUrl": null
  },
  "log": []
}
```

For more information see the following:
  * [Livy API](./Livy-api.html).
  * [Spark App in Object Store](./Spark-App-in-Object-Store.html).

---

## SSH

You can run spark-submit by logging to the cluster using [SSH](./Connect-using-SSH.html).

1. Log on to cluster management node.

  ```
  $ ssh clsadmin@iae-tmp-867-mn003.bi.services.us-south.bluemix.net
  ```

2. Run spark-submit.

  ```
  $ spark-submit \
  --master yarn \
  --deploy-mode cluster \
  --class org.apache.spark.examples.SparkPi \
  /usr/iop/current/spark2-client/jars/spark-examples.jar
  ```

For more information see the following:
  * [Connect using SSH](./Connect-using-SSH.html).
  * [Apache Spark Submitting Applications](http://spark.apache.org/docs/latest/submitting-applications.html).
