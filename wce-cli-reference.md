---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Analytics Engine command line interface

The {{site.data.keyword.Bluemix_notm}} command line interface can be used to interact with the {{site.data.keyword.iae_full_notm}} cluster.

# Getting Started

- For details, see [Installing the {{site.data.keyword.iae_short}} CLI plugin](./wce-wcl-install.html).

# Usage

```
$ bx iae
NAME:
   bx iae - Watson Compute Engine commands
USAGE:
   bx iae command [arguments...] [command options]

COMMANDS:
   file-system            Interact with HDFS on {{site.data.keyword.iae_short}} cluster
   spark-endpoint         Set the server endpoint
   spark-job-cancel       Cancel a Spark Job submitted on the {{site.data.keyword.iae_short}} cluster
   spark-job-status       Retrieve the status of the Spark job from the {{site.data.keyword.iae_short}} cluster
   spark-job-statuses     Retrieve the status of the Spark job from the {{site.data.keyword.iae_short}} cluster
   spark-logs             Get spark job logs
   spark-submit           Submit a Spark job to the {{site.data.keyword.iae_short}} cluster
   username               Set the default username for {{site.data.keyword.iae_short}} commands.
   versions               Get the versions of the services running in {{site.data.keyword.iae_short}} cluster
   help

Enter 'bx iae help [command]' for more information about a command.
```

# Command Reference

- [file-system](./wce-cli-ref-file-system.html)
- [spark-endpoint](./wce-cli-ref-spark-endpoint.html)
- [spark-job-cancel](./wce-cli-ref-spark-job-cancel.html)
- [spark-job-status](./wce-cli-ref-spark-job-status.html)
- [spark-job-statuses](./wce-cli-ref-spark-job-statuses.html)
- [spark-logs](./wce-cli-ref-spark-logs.html)
- [spark-submit](./wce-cli-ref-spark-submit.html)
- [username](./wce-cli-ref-username.html)
- [versions](./wce-cli-ref-versions.html)

# FAQs

- [How to submit a job?](./Spark-Batch.html)

# Troubleshoot

- [Enable tracing](./WCE-CLI-Troubleshoot.html#enable-tracing)
- [Endpoint was not set or found](./WCE-CLI-Troubleshoot.html#endpoint-was-not-set-or-found-call-endpoint-first)
