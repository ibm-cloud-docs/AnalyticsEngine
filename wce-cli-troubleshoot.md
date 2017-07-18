---

copyright:
  years: 2017
lastupdated: "2017-07-13"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

#  Analytics Engine command line interface troubleshooting
## Enable tracing

Tracing can be enabled by setting `BLUEMIX_TRACE` environment variable to `true` (case ignored). When trace enabled additional debugging information will be printed on the terminal.

On Linux/macOS terminal

```
$ export BLUEMIX_TRACE=true
```

On Windows prompt

```
SET BLUEMIX_TRACE=true
```

To disable tracing set `BLUEMIX_TRACE` environment variable to `false` (case ignored)

## Endpoint was not set or found. Call endpoint first.

The Analytics Engine command line interface requires a cluster endpoint to be first set. This enables the tool to talk to the cluster. The endpoint is the ip or hostname of the management node.

To set the cluster endpoint:

```
$ bx ae spark-endpoint https://169.54.195.210
Registering endpoint 'https://169.54.195.210'...
Ambari Port Number [Optional: Press enter for default value] (9443)>
Knox Port Number [Optional: Press enter for default value] (8443)>
OK
Endpoint 'https://169.54.195.210' set.
```
