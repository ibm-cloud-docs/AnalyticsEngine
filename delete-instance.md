---

copyright:
  years: 2017
lastupdated: "2017-08-04"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Deleting a service instance

You can delete a service instance by using the Bluemix user interface, the Cloud Foundry CLI, or the Cloud Foundry REST API. The underlying cluster gets deleted as part of service instance deletion. Any data and metadata, including logs, in the cluster will be lost once you delete the cluster.

## Bluemix UI
a. Navigate to your organization's dashboard page [https://console.ng.bluemix.net/dashboard/services](https://console.ng.bluemix.net/dashboard/services) and switch to the space where you had created the service instance.  
b. From the service instance's Action menu choose 'Delete Service'

## cf CLI

**Pre-requisite**: If you have any service keys for your service instance, you need to delete them first, before attempting to delete the service instance.

```
cf api https://api.ng.bluemix.net
cf login
<choose your org and space>
cf delete-service <service_instance_name>
```
{: codeblock}

## cf REST API

```
curl --request DELETE \
  --url 'https://api.ng.bluemix.net/v2/service_instances/<service_instance_id>' \
  --header 'authorization: <User's bearer token>' \
```
{: codeblock}
