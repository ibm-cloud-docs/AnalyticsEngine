---

copyright:
  years: 2017
lastupdated: "2017-07-23"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Installing the Analytics Engine command line interface

## Prerequisites
To use the IBM Analytics Engine  command line interface, download and install the following packages on local machine. Do not install the packages on the cluster.
 
- The [Cloud Foundry CLI](https://github.com/cloudfoundry/cli/blob/master/README.md#installing-using-a-package-manager)

- The [{{site.data.keyword.Bluemix_notm}} Engine CLI](https://console.bluemix.net/docs/cli/index.html#cli)

## Installing the Analytics Engine CLI
For details about {{site.data.keyword.Bluemix_notm}} CLI plugin installation, see the [documentation](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started).

To list the plugins in the {{site.data.keyword.Bluemix_notm}} repository:

```
bx plugin repo-plugins -r Bluemix
```
{: codeblock}

To install the plugin from the {{site.data.keyword.Bluemix_notm}} repository:

```
bx plugin install -r Bluemix analytics-engine
```
{: codeblock}

Note: If the default {{site.data.keyword.Bluemix_notm}} repository is not available from your {{site.data.keyword.Bluemix_notm}} CLI, you might need to add the repository. This only needs to be done once.
 
```
bx plugin repo-add Bluemix https://plugins.ng.bluemix.net
```
{: codeblock}


## Uninstalling the Analytics Engine CLI
Run the following command to uninstall the command line interface:

```
$ bx plugin uninstall analytics-engine 
  Uninstalling plug-in 'ae'...
  OK
  Plug-in 'ae' was successfully uninstalled.
```
{: codeblock}

## Updating the Analytics Engine CLI

Perform the following steps to update the command line interface:

1. Uninstall the existing CLI plugin.
3. Install the new Analytics Engine CLI plugin.
