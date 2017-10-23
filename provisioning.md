---

copyright:
  years: 2017
lastupdated: "2017-09-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Provisioning an Analytics Engine service instance

You can create an Analytics Engine service instance through one of the following ways:

* [From Bluemix console](#creating-a-service-instance-from-bluemix-console)
* [Using the Cloud Foundry (cf) command-line interface (CLI)](#creating-a-service-instance-using-the-cloud-foundry-command-line-interface)
* [Using the Cloud Foundry (cf) REST API](#creating-a-service-instance-using-the-cloud-foundry-rest-api)

**Pre-requisite**: You must have access to Bluemix US-South region.

**Restriction**: Currently, in each Bluemix organization, you can create at most one service instance (one cluster) with a maximum of three compute nodes.

## Creating a service instance from Bluemix console

1. Log into Bluemix console: [https://console.ng.bluemix.net](https://console.ng.bluemix.net).
Once logged in, make sure that you have chosen `US South` as the region (on top right corner of the console), choose the Bluemix organization and where you have access to create service instances.

2. Click the following link to open the service instance creation page: [Analytics Engine](https://console.ng.bluemix.net/catalog/services/ibm-analytics-engine?env_id=ibm:yp:us-south&taxonomyNavigation=apps).

3. Specify the number of compute nodes you require, choose a `Software package` and click **`Create`**.

**Restrictions**:

* You can specify a maximum of three compute nodes.
* During beta, your service instance expires within 7 days of creation. If you want to work with the service again after the previous service instance expired, you can delete the expired service instance and create a new service.

**Software packages**:

* Choose _`AE 1.0 Spark`_ if you are planning to run only Spark workloads.
* Choose _`AE 1.0 Hadoop and Spark`_ if you are planning to run Hadoop workloads in addition to Spark workloads. In addition to the components you get with Spark package, you also get Oozie, HBase and Hive, as part of the components of the Hadoop package.

## Creating a service instance using the Cloud Foundry Command Line Interface

* Download the Cloud Foundry CLI from [here](https://github.com/cloudfoundry/cli#downloads).

### First-time setup:

1. Install the Cloud Foundry CLI by running the [downloaded installer](https://github.com/cloudfoundry/cli#downloads).
2. To set the API end point and log in, enter:
```
cf api https://api.ng.bluemix.net
cf login
```
3. Enter your Bluemix credentials. When prompted, choose your organization and space.

### Creating a service instance:
```
cf create-service IBMAnalyticsEngine Standard <service instance name> -c <cluster parameters as json string enclosed in single quotes or path to cluster parameters json file>
```
{: codeblock}

Sample cluster parameters json file  
```
{
        "num_compute_nodes": 1,
        "hardware_config": "Standard",
        "software_package": "ae-1.0-spark"
}
```
{: codeblock}

### Brief description of cluster parameters
1. **`num_compute_nodes`** (Required): Number of compute nodes required in the cluster. Max value: _`3`_   
2. **`hardware_config`** (Required): Represents the instance size of the cluster. Accepted value: _`Standard`_  
3. **`software_package`** (Required): Determines set of services to be installed on the cluster. Accepted value: _`ae-1.0-spark`_ and _`ae-1.0-hadoop-spark`_
4. **`customization`** (Optional): Array of customization actions to be run on all nodes of the cluster once it is created. At the moment, only one customization action can be specified. The various types of customization actions that can be specified are discussed in detail in [Customizing clusters](./customizing-cluster.html).
<br>

**cf CLI Response** <br>
```
Create in progress. Use `cf services` or `cf service <service instance name>` to check operation status.
```
Service provisioning happens asynchronously. Successful response just means that the provisioning request has been accepted. Service provisioning is considered complete only when the associated cluster is created and made Active.

## Querying for service provisioning status
Command - `cf service <service instance name>`
Sample Response -
```
Service instance: MYSERVICE1
Service: IBMAnalyticsEngine
Bound apps:
Tags:
Plan: Standard

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
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"febf38af-bb11-4d55-8732-49a9b67a480f", "parameters": { "hardware_config":"Standard", "num_compute_nodes":1, "software_package":"ae-1.0-spark"}}'
```
{: codeblock}

## Creating a service instance using the Cloud Foundry REST API
Use the following information to create a service instance:
* service_plan_guid to use: `febf38af-bb11-4d55-8732-49a9b67a480f`
* cf API end point: `https://api.ng.bluemix.net/v2`

*Response:*
The response is in JSON format.
If the create cluster request is accepted, the property `metadata.guid` has the new service instance's ID.
If the request is rejected, the property `description` contains a helpful message.

### Via Cloud Foundry REST API
```
curl --request GET \
  --url https://api.ng.bluemix.net/v2/service_instances/<service_instance_guid> \
  --header 'accept: application/json' \
  --header 'authorization: bearer <user's UAA bearer token>'
```
{: codeblock}

**Note**: If the service instance failed to be created, delete the service instance and try creating it again. If the problem persists, contact IBM Support.
