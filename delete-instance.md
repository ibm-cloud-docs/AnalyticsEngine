---

copyright:
  years: 2017
lastupdated: "2017-07-16"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Deleting a service instance

You can delete a service instance by using the Bluemix user interface, the Cloudfoundry CLI, or the Cloudfoundry REST API. The underlying cluster gets deleted as part of service instance deletion. Any data and metadata, including logs, in the cluster will be lost once you delete the cluster.

## Bluemix UI
a. Navigate to your organization's dashboard page [https://console.ng.bluemix.net/dashboard/services](https://console.ng.bluemix.net/dashboard/services) and switch to the space where you had created the service instance.  
b. From the service instance's Action menu choose 'Delete Service'

## CF CLI

**Pre-requisite**: If you have any service keys for your service instance, you need to delete them first, before attempting to unprovision the service instance.

```
cf api https://api.ng.bluemix.net
cf login
<choose your org and space>
cf delete-service <service_instance_name>
```

## CF REST API

```
curl --request DELETE \
  --url 'https://api.ng.bluemix.net/v2/service_instances/<service_instance_id>' \
  --header 'authorization: <User's bearer token>' \
```
