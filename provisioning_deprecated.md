---

copyright:
  years: 2017,2018
lastupdated: "2018-03-15"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Provisioning an Analytics Engine service instance using Cloud Foundry (deprecated)

Creating an Analytics Engine service instance by using Cloud Foundry is deprecated. You should now use the [Resource Controller option](./provisioning.html) to provision instances.

In Cloud Foundry, you can provision an Analytics Engine service instance through one of the following ways:

* [Using the Cloud Foundry (cf) command-line interface (CLI)](#creating-a-service-instance-using-the-cloud-foundry-command-line-interface)
* [Using the Cloud Foundry (cf) REST API](#creating-a-service-instance-using-the-cloud-foundry-rest-api)

**Prerequisite**: You must have access to the {{site.data.keyword.Bluemix_short}} US-South region.

## Creating a service instance using the Cloud Foundry Command Line Interface

* Download the Cloud Foundry CLI from [here](https://github.com/cloudfoundry/cli#downloads).

### First-time setup:

1. Install the Cloud Foundry CLI by running the [downloaded installer](https://github.com/cloudfoundry/cli#downloads).
2. To set the API end point and log in, enter:
```
cf api https://api.ng.bluemix.net
cf login
```
3. Enter your IBM Cloud credentials. When prompted, choose your organization and space.

### Creating a service instance:
```
cf create-service IBMAnalyticsEngine <Plan name> <service instance name> -c <path to JSON   file with cluster parameters>
```

For example:

```
cf create-service IBMAnalyticsEngine lite MyServiceInstance -c /usr/testuser/cluster_specification.json
```

Supported plan names are lite, standard-hourly, and standard-monthly.

Sample cluster specification JSON file  
```
{
        "num_compute_nodes": 1,
        "hardware_config": "default",
        "software_package": "ae-1.0-spark"
}
```
{: codeblock}

### Brief description of cluster specification parameters

1. **`num_compute_nodes`** (Required): Number of compute nodes required in the cluster.
2. **`hardware_config`** (Required): Represents the instance size of the cluster. Accepted value: _`default, -memory-intensive`_  
3. **`software_package`** (Required): Determines set of services to be installed on the cluster. Accepted value: _`ae-1.0-spark`_ and _`ae-1.0-hadoop-spark`_
4. **`customization`** (Optional): Array of customization actions to be run on all nodes of the cluster once it is created. At the moment, only one customization action can be specified. The various types of customization actions that can be specified are discussed in detail in [Customizing clusters](./customizing-cluster.html).
<br>

**cf CLI Response:** <br>
```
Create in progress. Use `cf services` or `cf service <service instance name>` to check operation status.
```
Service provisioning happens asynchronously. Successful response just means that the provisioning request has been accepted. Service provisioning is considered complete only when the associated cluster is created and made Active.

## Provisioning overview

For an overview of how a cluster is provisioned and how to check your cluster provisioning state, see [Track cluster provisioning](./track-instance-provisioning.html).

## Querying for service provisioning status
Command - `cf service <service instance name>`

Sample response:

```
Service instance: MyServiceInstance
Service: IBMAnalyticsEngine
Bound apps:
Tags:
Plan: Lite

Last Operation
Status: create in progress
Message:
Started: 2017-04-04T21:13:40Z
Updated:

Note: 'The Last Operation' section indicates the service provisioning status. When provisioning is ongoing, it's 'create in progress'. When provisioning has completed, it will be 'create succeeded'.
```

### Obtaining the space GUID

The output returned by the following command is the space GUID that you would use in the REST API calls in the following section.

**To get the space GUID**

* Log into cf CLI and run the command:
```
cf target -o <your organization name> -s <your space name>
cf space --guid <your space name>
```
Create the service request url: `https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true`

Usage:

```
Create the cluster without customization

  curl --request POST \
  --url 'https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true' \
  --header 'accept: application/json' \
  --header 'authorization: Bearer <User's bearer token>' \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/json' \
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"acb06a56-fab1-4cb1-a178-c811bc676164", "parameters": { "hardware_config":"default", "num_compute_nodes":1, "software_package":"ae-1.0-spark"}}'
```
{: codeblock}

## Creating a service instance using the Cloud Foundry REST API

To invoke the Cloud Foundry REST API to create a service instance, you need your space GUID.

To get the space GUID, log into cf CLI and run the command:
```
cf target -o <your organization name> -s <your space name>  
cf space --guid <your space name> ```


You need the  following information to create a service instance:

•	The `service_plan_guid`:

	  * For Lite use: `acb06a56-fab1-4cb1-a178-c811bc676164`
	  * For Standard-Hourly use: `9ba7e645-fce1-46ad-90dc-12655bc45f9e`
	  * For Standard-Monthly use: `f801e166-2c73-4189-8ebb-ef7c1b586709`


*Response:*
The response is in JSON format. If the create cluster request is accepted, the property `metadata.guid` has the new service instance's ID. If the request is rejected, the property `description` contains a helpful message.

### Via Cloud Foundry REST API
To create an instance via the cf Rest API enter:
```
curl --request GET \
  --url https://api.ng.bluemix.net/v2/service_instances/<service_instance_guid> \
  --header 'accept: application/json' \
  --header 'authorization: bearer <user's UAA bearer token>'
```
{: codeblock}

**Note**: If the service instance was not successfully created, delete the service instance and try creating it again. If the problem persists, contact IBM Support.

## Plan upgrading

You can only upgrade from a Lite plan to a Standard-Hourly plan.

To upgrade using the cf CLI:

1. Enter the following command:

	```
	cf update-service <your tileservice instance name> -p standard-hourly
	```
