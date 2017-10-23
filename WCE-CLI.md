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

# Analytics Engine command line interface

The Bluemix Analytics Engine command line interface can be used to interact with the IBM Analytics Engine cluster.


## Getting Started

- [Install the CLI plugin](./wce-wcl-install.html)

## Usage

```
$ bx ae
NAME:
   bx ae - IBM Analytics Engine commands
USAGE:
   bx ae command [arguments...] [command options]

COMMANDS:
   endpoint             Set the server endpoint
   file-system          Interact with HDFS on IBM  Analytics Engine cluster
   kernels              Interact with kernels on IBM Analytics Engine cluster
   spark-job-cancel     Cancel a Spark Job submitted on the IBM Analytics Engine cluster
   spark-job-status     Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-job-statuses   Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-logs           Get spark job logs
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


