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

# Troubleshooting

You can find the answers to common questions about how to use IBM Analytics Engine.

## Jupyter Kernel Gateway

### When the notebook user interface opens, the kernel remains in a busy state (filled circle) although no code is running

 The notebook kernel is in busy state although no code cells are running because lazy evaluation could not be initialized on the Spark cluster and no more YARN containers are available on the cluster.

You can verify this by checking that the 'state' of your application is 'ACCEPTED' in the YARN RM UI.

To fix this issue, stop the existing running notebook kernels or any other YARN applications to free up resources.


## Cluster management

### When I open the cluster management page, an error message stating that I am not authorized appears

You might not be able to access the cluster management page for the following reasons:

1) You do not have developer access to the {{site.data.keyword.Bluemix_notm}} space.

2) Cookies are not enabled in your browser.

To fix this issue:

1) Work with the manager of your {{site.data.keyword.Bluemix_notm}} organization or space and get yourself developer privilege to the space containing the service instance you are attempting to access.

2) Ensure that cookies are enabled in your browser.

## Command line interface

### Enable tracing in the command line interface

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

### Endpoint was not set or found. Call endpoint first.

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
