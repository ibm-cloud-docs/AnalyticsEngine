<<<<<<< HEAD
---

copyright:
  years: 2017
lastupdated: "2017-07-31"

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

**Restriction**: Currently, in each Bluemix space, you can create at most one service instance (one cluster) with a maximum of three compute nodes.

## Creating a service instance from Bluemix console

1. Log into Bluemix console: [https://console.ng.bluemix.net](https://console.ng.bluemix.net).
Once logged in, make sure that you have chosen `US South` as the region (on top right corner of the console), choose the Bluemix organization and the space name allotted to you.

2. Click the following link to open the service instance creation page: [IBM Analytics Engine](https://console.ng.bluemix.net/catalog/services/ibm-analytics-engine?env_id=ibm:yp:us-south&taxonomyNavigation=apps).

3. Specify the number of compute nodes you require, choose a `Software package` and click **`Create`**.

**Restriction**: You can specify a maximum of three compute nodes.

**Software packages**:
* Choose _`ae-1.0-SparkPack`_ , if you are planning to run only Spark workloads.
* Choose _`ae-1.0-HadoopPack`_ , if you are planning to run Hadoop workloads in addition to Spark workloads. In addition to the components you get with Spark pack, you also get Oozie, HBase and Hive, as part of the components of the Hadoop pack.

## Creating a service instance using the Cloud Foundry Command Line Interface

* Use this link to download the cf CLI: https://github.com/cloudfoundry/cli#downloads.

### First-time setup:

1. Install cf CLI by running the [downloaded installer](https://github.com/cloudfoundry/cli#downloads).
2. Set the API end point and log in.
```
cf api https://api.ng.bluemix.net
cf login
```
3. Enter your Bluemix credentials. When prompted, choose your organization and space.

### Creating a service instance:
```
cf create-service IBMAnalyticsEngine Lite <service instance name> -c <cluster parameters as json string enclosed in single quotes or path to cluster parameters json file>
```
{: codeblock}

Sample cluster parameters json file  
```
{
        "num_compute_nodes": 1,
        "hardware_config": "Standard",
        "software_package": "ae-1.0-SparkPack"
}
```
{: codeblock}

### Brief description of cluster parameters
1. **`num_compute_nodes`** (Required): Number of compute nodes required in the cluster. Max value: _`2`_   
2. **`hardware_config`** (Required): Represents the instance size of the cluster. Accepted value: _`Standard`_  
3. **`software_package`** (Required): Determines set of services to be installed on the cluster. Accepted value: _`ae-1.0-SparkPack`_ and _`ae-1.0-HadoopPack`_
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
Plan: Lite

Last Operation
Status: create in progress
Message:
Started: 2017-04-04T21:13:40Z
Updated:

Note: 'The Last Operation' section indicates the service provisioning status. When provisioning is ongoing, it's 'create in progress'. When provisioning has completed, it will be 'create succeeded'.
```

### Obtaining the Bearer Token for API authentication

* Log in to `cf` CLI and run the command `cf oauth-token`. The output of this command is the UAA access token to be passed to CF REST APIs for creating a service instance.

**Very Important:** You should not share this token with other users. Use this as the value for request header 'authorization' in your cloudfoundry REST API calls.

### Obtaining the space GUID

The output returned by the following command is the space GUID that you would use in the REST API calls in the following section.

**To get the space GUID**

* Log into cf CLI and run the command:
```
cf target -o <your organization name> -s <your space name>
cf space --guid <your space name>
```
{: codeblock}

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
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"febf38af-bb11-4d55-8732-49a9b67a480f", "parameters": { "hardware_config":"Standard", "num_compute_nodes":1, "software_package":"ae-1.0-SparkPack"}}'
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
=======
---

copyright:
  years: 2017
lastupdated: "2017-07-17"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Provisioning a service instance

You can create a service instance through one of the following ways:

* [From Bluemix console](#creating-a-service-instance-from-bluemix-console)
* [Using Cloudfoundry CLI](#creating-a-service-instance-using-cloud-foundry-command-line-interface)
* [Using Cloudfoundry REST API](#creating-a-service-instance-using-cloud-foundry-rest-api)

**Pre-requisite**: You must have access to Bluemix US-South region.

**Restriction**: Currently, in each Bluemix space, you can create at most one service instance (one cluster) with a maximum of two compute nodes.

## Creating a service instance from Bluemix console

1. Log into Bluemix console: [https://console.ng.bluemix.net](https://console.ng.bluemix.net).
Once logged in, make sure that you have chosen `US South` as the region (on top right corner of the console), `IAEDEV` as the org and the space name allotted to you.

2. Click the following link to open the service instance creation page: [IBM Analytics Engine](https://console.ng.bluemix.net/catalog/services/ibm-analytics-engine?env_id=ibm:yp:us-south&taxonomyNavigation=apps). 

3. Specify the number of compute nodes you require, choose a `Software package` and click **`Create`**.

**Restriction**: You can specify a maximum of two compute nodes.

**Software packages**:
* Choose _`ae-1.0-SparkPack`_ , if you are planning to run only Spark workloads.
* Choose _`ae-1.0-HadoopPack`_ , if you are planning to run Hadoop workloads in addition to Spark workloads. In addition to the components you get with Spark pack, you also get Oozie, HBase and Hive, as part of the components of the Spark pack.
 
## Creating a service instance using Cloudfoundry Command Line Interface
Link to download CF CLI: https://github.com/cloudfoundry/cli#downloads

### First-time setup:
1. Install cf CLI by running the [downloaded installer](https://github.com/cloudfoundry/cli#downloads).
2. Set API end point and log in.
```
cf api https://api.ng.bluemix.net
cf login
```
{: codeblock}

Enter your Bluemix credentials. When prompted, choose your organization and space.

### Creating a service instance:
```
cf create-service IBMAnalyticsEngine Lite <service instance name> -c <cluster parameters as json string or path to cluster parameters json file>
```
{: codeblock}

Sample cluster parameters json file  
```
{
        "num_compute_nodes": 1,
        "hardware_config": "Standard",
        "software_package": "ae-1.0-SparkPack"
}
```
{: codeblock}

### Brief description of cluster parameters 
1. **`num_compute_nodes`** (Required): Number of compute nodes required in the cluster. Max value: _`2`_   
2. **`hardware_config`** (Required): Represents the instance size of the cluster. Accepted value: _`Standard`_  
3. **`software_package`** (Required): Determines set of services to be installed on the cluster. Accepted value: _`ae-0.1-SparkPack`_ and _`ae-0.1-HadoopPack`_ 
4. **`customization`** (Optional): Array of customization actions to be run on all nodes of the cluster once it is created. At the moment, only one customization action can be specified. The various types of customization actions that can be specified are discussed in detail in [Customizing clusters](./customizing-cluster.html). 
<br>

**cf CLI Response** <br>
```
Create in progress. Use `cf services` or `cf service <service instance name>` to check operation status.
```
Service provisioning happens asynchronously. Successful response just means that the provisioning request has been accepted. Service provisioning is considered complete only when the associated cluster is created and made Active.

## Creating a service instance using Cloudfoundry REST API
*service_plan_guid to use:* `fd6700e7-f8ba-4000-bb24-b1a63ce45d3a`
*CF API end point:* `https://api.stage1.ng.bluemix.net/v2`
### Obtaining the Bearer Token for API authentication:
* Log in to `cf` CLI and run the command `cf oauth-token`. The output of this command is the UAA access token to be passed to CF REST APIs for creating a service instance.
**Very Important:** You should not share this token with other users. Use this as the value for request header 'authorization' in your cloudfoundry REST API calls.

Create service request url: `https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true`

Usage:

```
1. Create cluster without customization

  curl --request POST \
  --url 'https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true' \
  --header 'accept: application/json' \
  --header 'authorization: <User's bearer token>' \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/json' \
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"fd6700e7-f8ba-4000-bb24-b1a63ce45d3a", "parameters": { "hardware_config":"Standard", "num_compute_nodes":1, "software_package":"ae-0.1-SparkPack"}}'
```
{: codeblock}

*Response:*
The respnse is in JSON format. 
If the create cluster request is accepted, the property `metadata.guid` has the new service instance's ID.
If the request is rejected, the property `description` contains a helpful message.

## Querying for service provisioning status
### Via cf CLI
Command - `cf service <service instance name>`
Sample Response -
```
Service instance: MYSERVICE1
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

### Via CloudFoundry REST API
```
curl --request GET \
  --url https://api.ng.bluemix.net/v2/service_instances/<service_instance_guid> \
  --header 'accept: application/json' \
  --header 'authorization: <user's UAA bearer token>' 

```
{: codeblock}
>>>>>>> origin/master
