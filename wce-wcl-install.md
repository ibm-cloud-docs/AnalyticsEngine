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

## Pre-Requisites

- Install the [Cloud Foundry CLI](https://github.com/cloudfoundry/cli/blob/master/README.md#installing-using-a-package-manager)

- Install the [Bluemix Engine CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/index.html#install_bluemix_cli)

## Installing the Analytics Engine CLI
For details about Bluemix CLI plugin installation, see the [documentation](https://console.bluemix.net/docs/cli/reference/bluemix_cli/index.html#install_plug-in).

To list the plugins in the Bluemix repository:

```
bx plugin repo-plugins -r Bluemix
```
{: codeblock}

To install the plugin from the Bluemix repository:

```
bx plugin install -r Bluemix analytics-engine
```
{: codeblock}

Note: If the default Bluemix repository is not available from your Bluemix CLI, you might need to add the repository. This only needs to be done once.
 
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
