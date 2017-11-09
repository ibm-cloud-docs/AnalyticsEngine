---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Provisioning an Analytics Engine service instance

You can create an Analytics Engine service instance through one of the following ways:

* [From the {{site.data.keyword.Bluemix_short}} console](#creating-a-service-instance-from-bluemix-console)
* [Using the Cloud Foundry (cf) command-line interface (CLI)](#creating-a-service-instance-using-the-cloud-foundry-command-line-interface)
* [Using the Cloud Foundry (cf) REST API](#creating-a-service-instance-using-the-cloud-foundry-rest-api)

**Prerequisite**: You must have access to the {{site.data.keyword.Bluemix_short}} US-South region.

## Creating a service instance from the   {{site.data.keyword.Bluemix_notm}} console

1. Log into the {{site.data.keyword.Bluemix_short}} console: [https://console.ng.bluemix.net](https://console.ng.bluemix.net).
Once logged in, make sure that you have chosen `US South` as the region (on top right corner of the console), choose the {{site.data.keyword.Bluemix_short}} organization and where you have access to create service instances.
1. Click the following link to open the service instance creation page: [Analytics Engine](https://console.ng.bluemix.net/catalog/services/ibm-analytics-engine?env_id=ibm:yp:us-south&taxonomyNavigation=apps).
1. Select a plan and click **`Configure`**.
1. On the configuration page, choose the hardware configuration, number of compute nodes and software package of your choice. Click **Create**.
1. The service instance is provisioned in the background and can take anywhere between 10 to 20 minutes to provision depending on the hardware type and software package you chose. Visit the {{site.data.keyword.Bluemix_short}} page after some time to check the status of the provisioned instance.

### Supported plans

| Plan | Hardware types | Software packages | Restrictions |
|------|----------------|-------------------|------------- |
| Lite | Default | AE 1.0 Spark, AE 1.0 Spark and Hadoop | 1.	Maximum of one tile per IBM Cloud organization every 30 days </br> 2.	Maximum of one cluster with up to 3 compute nodes </br> 3.	After 50 node hours, the cluster will be disabled. During disable period, the cluster cannot be scaled up or customized. A grace period of 24 hours is given for the user to upgrade his account to a Paid account, and to upgrade the service instance to Standard-Hourly plan. </br> If the service instance is not upgraded, then it will expire and be deleted.|
| Standard-Hourly | Default, memory intensive |	AE 1.0 Spark, AE 1.0 Spark and Hadoop | NA |
| Standard-Monthly | Default, memory intensive | AE 1.0 Spark, AE 1.0 Spark and Hadoop | NA |

Hardware specifications:

 - 	Default: 4 vCPU, 16 GB RAM, 2 x 300 GB HDFS disk on each compute node
 -	Memory intensive: 32 vCPU, 128 GB RAM, 3 x 300 GB HDFS disk on each compute node

Software packages:

 - Choose AE 1.0 Spark if you are planning to run only Spark workloads.
 - Choose AE 1.0 Hadoop and Spark if you are planning to run Hadoop workloads in addition to Spark workloads. In addition to the components you get with the Spark package, you also get Oozie, HBase and Hive, as part of the components of the Hadoop package.


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

**cf CLI Response** <br>
```
Create in progress. Use `cf services` or `cf service <service instance name>` to check operation status.
```
Service provisioning happens asynchronously. Successful response just means that the provisioning request has been accepted. Service provisioning is considered complete only when the associated cluster is created and made Active.

## Querying for service provisioning status
Command - `cf service <service instance name>`
Sample Response -
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

You can only upgrade from a Lite plan to a Standard-Hourly plan. You can upgrade by using the IBM Analytics Engine UI or by using Cloud Foundry command line interface.

To upgrade a Lite plan using the Analytics Engine dashboard in IBM Cloud:

1. Open your IBM Analytics Engine service dashboard page.

1. Select the **Plan** tab and then select the Standard-Hourly plan.

1. Click Save.

To upgrade using the cf CLI:

1. Enter the following command:

	```
	cf update-service <your tileservice instance name> -p standard-hourly
	```
