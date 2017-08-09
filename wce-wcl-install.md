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

## Installing from the public repository
To list the plugins in the Bluemix repository:

```
bx plugin repo-plugins -r Bluemix
```
{: codeblock}

To install the plugin from Bluemix repository:

```
bx plugin install -r Bluemix analytics-engine
```
{: codeblock}

## Installing from the staging repository (Stage 1)

To see the plugins, add the staging repository (this only needs to be done once):

```
bx plugin repo-add BluemixStage1 https://plugins.stage1.ng.bluemix.net
```
{: codeblock}

To view the plugin repositories:

```
bx plugin repos
```
{: codeblock}

To list the plugins in the staging repository:

```
bx plugin repo-plugins -r BluemixStage1
```
{: codeblock}

To install the plugin from the staging repository:

```
bx plugin install -r BluemixStage1 analytics-engine
```
{: codeblock}

## Installing from the internal build
Perform the following steps to install the command line interface:

1. Download the Analytics Engine [build CLI](http://idc-nexus01.svl.ibm.com:8081/nexus/service/local/artifact/maven/redirect?r=wdp-chs-snapshot&g=com.ibm.wdp.chs&a=spark-cli&e=tgz&v=LATEST).

2. Extract the downloaded tar file and run the following command:

  ```
  bluemix plugin install <path to plugin>
  ```
{: codeblock}

  On Windows:

  ```
  bx plugin install C:\Temp\Zip\spark-cli-0.0.76-SNAPSHOT\bin\windows_amd64\spark-cli.exe
  ```
{: codeblock}

  On Linux:

  ```
  bx plugin install /home/user/Downloads/spark-cli-0.0.76-SNAPSHOT/bin/linux/spark-cli
  ```
{: codeblock}

  On macOS:

  ```
  $bx plugin install /home/user/Downloads/spark-cli-0.0.76-SNAPSHOT/bin/darwin_amd64/spark-cli
  Installing plugin...
  OK
  Plug-in 'ae 0.0.0' was successfully installed into /Users/jeyaramashokraj/.bluemix/plugins/ae.
  ```
{: codeblock}

## Uninstalling the Analytics Engine CLI
Run the following command to uninstall the command line interface:

```
$ bx plugin uninstall ae
  Uninstalling plug-in 'ae'...
  OK
  Plug-in 'ae' was successfully uninstalled.
```
{: codeblock}

## Updating the Analytics Engine CLI

Perform the following steps to update the command line interface:

1. Download the new Analytics Engine CLI binary.
2. Uninstall the old CLI plugin.
3. Install the new Analytics Engine CLI from the downloaded location.
