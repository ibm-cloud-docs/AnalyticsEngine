---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-14"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Deleting a service instance
{: #delete-service}

You can delete a service instance by using one of the following methods:

- The [{{site.data.keyword.Bluemix_notm}} user interface](#ibm-cloud-user-interface)
- The [{{site.data.keyword.Bluemix_notm}} CLI](#ibm-cloud-cli)
- The [Resource Controller REST API](#resource-controller-rest-api)

The underlying cluster is deleted when the service instance is deleted. All data and metadata, including all logs, on the cluster will be lost after the cluster is deleted.

**Important**: {{site.data.keyword.iae_full_notm}} service instances, which were created before June 06 2018 in the Cloud Foundry organization and space, can be deleted only by using the {{site.data.keyword.Bluemix_notm}} user interface, the cf CLI, or the cf REST API.

## {{site.data.keyword.Bluemix_notm}} user interface

To delete an {{site.data.keyword.iae_full_notm}} instance by using the {{site.data.keyword.Bluemix_notm}} user interface:

1. Navigate to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://{DomainName}/dashboard/services) and select the {{site.data.keyword.Bluemix_notm}} service instance you want to delete.
1. From the service instance's Action menu, choose 'Delete Service'.

## {{site.data.keyword.Bluemix_notm}} CLI

**Prerequisite**: If you have any service keys for your service instance, you must delete them first, before attempting to delete the service instance.

To delete an {{site.data.keyword.iae_full_notm}} instance by using the {{site.data.keyword.Bluemix_notm}} CLI:

```
ibmcloud api https://api.ng.bluemix.net
ibmcloud login
<choose your account>
ibmcloud resource service-instance-delete <service_instance_name>
```
{: codeblock}

## Resource Controller REST API

**Prerequisite**: If you have any service keys for your service instance, you must delete them first, before attempting to delete the service instance. See [managing my IBM Cloud resources using the Resource Controller REST API](https://{DomainName}/apidocs/resource-controller) for more about deleting service keys.

To delete an {{site.data.keyword.iae_full_notm}} instance by using the Resource Controller REST API:

```
curl -X DELETE \
  https://resource-controller.bluemix.net/v2/resource_instances/<service_instance_id> \
  -H 'Authorization: Bearer <User's IAM access token>' \
```
{: codeblock}

To retrieve the IAM access token, see [Retrieving IAM access token](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).

## cf CLI (deprecated)

**Prerequisite**: If you have any service keys for your service instance, you must delete them first, before attempting to delete the service instance.

To delete an {{site.data.keyword.iae_full_notm}} instance by using the cf CLI:

```
cf api https://api.ng.bluemix.net
cf login
<choose your org and space>
cf delete-service <service_instance_name>
```

## cf REST API (deprecated)

To delete an {{site.data.keyword.iae_full_notm}} instance by using the cf REST API:

```
curl --request DELETE \
  --url 'https://api.ng.bluemix.net/v2/service_instances/<service_instance_id>' \
  --header 'authorization: <User's UAA access token>' \
```
To retrieve the UAA access token, see [Retrieving UAA access token](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-uaa-token).
