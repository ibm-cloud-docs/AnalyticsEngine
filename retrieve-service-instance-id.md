---

copyright:
  years: 2017,2018
lastupdated: "2018-03-07"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving the service instance ID

## Prerequisites

a. You must have a valid IBMid.

b. You must have installed the {{site.data.keyword.Bluemix_notm}} CLI.

## Step 1: Log into the {{site.data.keyword.Bluemix_notm}} CLI

To log into the CLI, enter the following commands:
```
bx api https://api.ng.bluemix.net
bx login <enter your credentials> ```

If you share an {{site.data.keyword.Bluemix_notm}} account, you'll be asked to choose an account for the current session.

## Step 2: List all service instances
Enter the following command to list all service instances:

```
bx resource service-instances ```

Sample response:
```
Retrieving service instances in resource group Default and all location under account ...
OK
Name                                     Location   State    Type              Tags
MyServiceInstance                        us-south   active   service_instance ```

## Step 3: Fetch the service instance ID

Enter the following command to retrieve the service instance ID:
```
bx resource service-instance MyServiceInstance --id ```

Sample response:
```
crn:v1:yp:public:rc-dev-siba1:us-south:a/217b381692bae8d7153823013f2cacf8:046a1611-81ff-4215-a5e0-6af8d7910610::```

Here `046a1611-81ff-4215-a5e0-6af8d7910610` is the service instance ID.
