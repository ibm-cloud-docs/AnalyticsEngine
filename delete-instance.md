---

copyright:
  years: 2017,2018
lastupdated: "2018-05-15"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Deleting a service instance

You can delete a service instance by using the {{site.data.keyword.Bluemix_notm}} user interface, the Cloud Foundry CLI, or the Cloud Foundry REST API. The underlying cluster gets deleted as part of service instance deletion. Any data and metadata, including logs, in the cluster will be lost once you delete the cluster.

## {{site.data.keyword.Bluemix_notm}} UI
a. Navigate to your organization's dashboard page [https://console.ng.bluemix.net/dashboard/services](https://console.ng.bluemix.net/dashboard/services) and switch to the space where you had created the service instance.  
b. From the service instance's Action menu choose 'Delete Service'

## cf CLI

**Prerequisite**: If you have any service keys for your service instance, you need to delete them first, before attempting to delete the service instance.

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
  --header 'authorization: <User's UAA bearer token>' \
```
{: codeblock}

**Note**: To retrieve the Cloud Foundry UAA access token, see [Retrieving the Cloud Foundry UAA access  token](./retrieving-uaa-access-token.html).
