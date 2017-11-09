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

# Analytics Engine command line interface

The {{site.data.keyword.Bluemix_notm}} Analytics Engine command line interface can be used to interact with the IBM Analytics Engine cluster.


## Getting Started

1. [Install the CLI plugin](./wce-wcl-install.html)
1. Run the [spark-endpoint](./wce-cli-ref-spark-endpoint.html) command to set the cluster endpoint. The argument for `endpoint` is the IP or hostname of the cluster management node.
  ```
  $ bx ae endpoint https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net
  ```

  	When prompted, accept the default port numbers for the Ambari(9443) and Knox(8443) services.

	  Response:
  ```
  Registering endpoint 'https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net'...
  Ambari Port Number [Optional: Press enter for default value] (9443)>
  Knox Port Number [Optional: Press enter for default value] (8443)>
  OK
  Endpoint 'https://ae-tmp-867-mn001.bi.services.us-south.bluemix.net' set.
  ```

1. Run the [spark-submit](./wce-cli-ref-spark-submit.html) command. For example:
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

## Usage

```
$ bx ae
NAME:
   bx ae - IBM Analytics Engine commands
USAGE:
   bx ae command [arguments...] [command options]

COMMANDS:
   endpoint             Set the server endpoint
   file-system          Interact with HDFS on IBM Analytics Engine cluster
   kernels              Interact with kernels on IBM Analytics Engine cluster
   spark-job-cancel     Cancel a Spark job submitted on the IBM Analytics Engine cluster
   spark-job-status     Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-job-statuses   Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-logs           Get Spark job logs
   spark-submit         Submit a Spark job to the IBM Analytics Engine cluster
   username             Set the default username for IBM Analytics Engine commands
   versions             Get the versions of the services running in IBM Analytics Engine cluster
   help                 


Enter 'bx ae help [command]' for more information about a command.
```

## Command Reference

- [endpoint](./wce-cli-ref-spark-endpoint.html)
- [file-system](./wce-cli-ref-file-system.html)
- [kernels](./wce-cli-ref-kernels.html)
- [spark-job-cancel](./wce-cli-ref-spark-job-cancel.html)
- [spark-job-status](./wce-cli-ref-spark-job-status.html)
- [spark-job-statuses](./wce-cli-ref-spark-job-statuses.html)
- [spark-logs](./wce-cli-ref-spark-logs.html)
- [spark-submit](./wce-cli-ref-spark-submit.html)
- [username](./wce-cli-ref-username.html)
- [versions](./wce-cli-ref-versions.html)

## FAQs

- [How to submit a job?](./Spark-Batch.html#wce-cli)

## Troubleshooting

- [Enable tracing](./wce-cli-troubleshoot.html#enable-tracing)
- [Endpoint was not set or found](./wce-cli-troubleshoot.html#endpoint-was-not-set-or-found-call-endpoint-first)
