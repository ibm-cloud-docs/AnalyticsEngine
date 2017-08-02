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

# Using a command line interface

You can use the Command Line Interface tool to interact with the IBM Analytics Engine cluster.

## Getting Started

- [Install the CLI plugin](./wce-wcl-install.html)

## Usage

```
$ bx ae
NAME:
   bx ae - Watson Compute Engine commands
USAGE:
   bx ae command [arguments...] [command options]

COMMANDS:
   file-system            Interact with HDFS on the IBM Analytics Engine cluster
   spark-endpoint         Set the server endpoint
   spark-job-cancel       Cancel a Spark Job submitted on the IBM Analytics Engine cluster
   spark-job-status       Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-job-statuses     Retrieve the status of the Spark job from the IBM Analytics Engine cluster
   spark-logs             Get spark job logs
   spark-submit           Submit a Spark job to the IBM Analytics Engine cluster
   username               Set the default username for IBM Analytics Engine commands.
   versions               Get the versions of the services running in the IBM Analytics Engine cluster
   help

Enter 'bx ae help [command]' for more information about a command.
```

## Command Reference

- [file-system](./wce-cli-ref-file-system.html)
- [spark-endpoint](./wce-cli-ref-spark-endpoint.html)
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


