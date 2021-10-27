---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-09"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:external: target="_blank" .external}

# Provisioning an {{site.data.keyword.iae_full_notm}} service instance
{: #provisioning-IAE}

You can create an {{site.data.keyword.iae_full_notm}} service instance through one of the following ways:

* [From the {{site.data.keyword.Bluemix_notm}} console](#creating-a-service-instance-from-the-ibm-cloud-console)
* [Using the command-line interface](#creating-a-service-instance-using-the-ibm-cloud-command-line-interface)
* [Using the Resource Controller REST API](#creating-a-service-instance-using-the-resource-controller-rest-api)

**Prerequisite**: You must have access to one of the following {{site.data.keyword.Bluemix_short}} regions (locations):

- `us-south` (Dallas)
- `us-east` (Washington DC)
- `eu-gb` (London)
- `eu-de` (Frankurt)
- `jp-tok` (Tokyo)
- `au-syd` (Sydney)

## Creating a service instance from the IBM Cloud console

To create an {{site.data.keyword.iae_full_notm}} instance:
1. Log into the [{{site.data.keyword.Bluemix_short}} console](https://{DomainName}){: external}.
1. Click **Create resource**, search for `{{site.data.keyword.iae_short}}` and then click on the tile to open the service instance creation page.
1. On the {{site.data.keyword.Bluemix_short}} catalog, choose the region in which you want the service instance to be deployed. {{site.data.keyword.iae_short}} deployments are available in US South, US East, United Kingdom, Germany, Japan, and Australia.
1. Choose the resource group under which you want to create the service instance.
1. Select a plan. With the `Standard-Hourly` and `Standard-Monthly` plans, you can choose how to access the {{site.data.keyword.iae_full_notm}} service endpoints.

  By default, a provisioned instance is accessible  over the public internet. However, you can also choose to open the {{site.data.keyword.iae_full_notm}} service endpoints over the {{site.data.keyword.Bluemix_short}} private network. See [Provisioning an instance with {{site.data.keyword.Bluemix_short}} service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).
  {: note}

  Select an option and click **Configure**.
1. On the configuration page, choose the hardware configuration, number of compute nodes and software package of your choice. Click **Create**.
1. The service instance is provisioned in the background and can take anywhere between 30 to 50 minutes to provision depending on the hardware type and software package you chose. Visit the {{site.data.keyword.Bluemix_short}} service details page after some time to check the status of the provisioned instance.

### Supported plans

{{site.data.keyword.iae_full_notm}} offers three plans: **Lite**, **Standard-Hourly**, and **Standard-Monthly**.  

An {{site.data.keyword.iae_short}} service instance comprises one cluster made up of one management node, N compute nodes and M task nodes, where N is the number of compute nodes and M is the number of task nodes that you specify when creating the cluster. A cluster with 3 compute nodes and 1 task node, for example, has 5 nodes in total and all 5 nodes are billed based on the billing plan.

A cluster must have at least one compute node but can have no task nodes.
{: note}


| Plan | Hardware types | Software packages        | Restrictions |
|------|----------------|------------------------------|--------- |
| **Lite** |**Default** |- `AE 1.2 Hive LLAP`  \n- `AE 1.2 Spark and Hive`  \n- `AE 1.2 Spark and Hadoop`  \n  | 1. This plan is available only to institutions that have signed up with IBM to try out the Lite plan. See [How does the Lite plan work](/docs/AnalyticsEngine?topic=AnalyticsEngine-general-faqs#free-usage)?  \n2. Maximum of one cluster with up to 1 compute node. No task node support.  \n3. Free usage limit is 50 node hours. This means that the cluster will be disabled after 25 hours.  \nA grace period of 24 hours is given to upgrade your user account to a paid account, and to upgrade the service instance to the Standard-Hourly plan.  \nIf the service instance is not upgraded, then it will expire and be deleted.  \n**Note:** You are entitled to one service instance per month. If you delete the service instance or it expires after the free 50 node hours, you will not be able to create a new one until after the month has passed.|
| **Standard-Hourly** | **Default** or **Memory intensive** | - `AE 1.2 Hive LLAP`  \n- `AE 1.2 Spark and Hive`  \n- `AE 1.2 Spark and Hadoop`  \n| NA |
| **Standard-Monthly** | **Default** or **memory intensive** | - `AE 1.2 Hive LLAP`  \n- `AE 1.2 Spark and Hive`  \n- `AE 1.2 Spark and Hadoop`  \n| NA |

Hardware specifications:

 - 	**Default**: 4 vCPU, 16 GB RAM, 2 x 300 GB HDFS disk on each compute node
 -	**Memory intensive**: 32 vCPU, 128 GB RAM, 3 x 300 GB HDFS disk on each compute node

Software packages:

The software packages on `AE 1.2` clusters include components for Horton Dataworks Platform 3.1. See [software packages](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#software){: new_window}.


### Provisioning recommendations

When provisioning a service instance:
- Choose the right plan; see [select the plan based on your workload use-case](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#plan){: new_window}.  
- Choose the appropriate hardware configuration; see [available hardware  configurations](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#hardware){: new_window}.
- Size the cluster appropriately; see [sizing the cluster](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#cluster-size){: new_window}.

## Creating a service instance using the IBM Cloud command-line interface

To create a service instance using the {{site.data.keyword.Bluemix_short}} command-line interface:

1. Download and configure the {{site.data.keyword.Bluemix_short}} CLI. Follow the instructions in [Getting started with the IBM Cloud CLI](/docs/cli?topic=cli-getting-started){: new_window}.

1. Set the API endpoint for your region and log in:
   ```
   ibmcloud api https://cloud.ibm.com
   ibmcloud login
   ```
   {: codeblock}

1. Create a service instance:
   ```
   ibmcloud resource service-instance-create <service instance name> ibmanalyticsengine <Plan name> <region> -p @<path to JSON file with cluster parameters>
   ```
   For example:
   ```
   ibmcloud resource service-instance-create MyServiceInstance ibmanalyticsengine lite us-south -p @/usr/testuser/cluster_specification.json
   ```
   Supported plan names are **lite**, **standard-hourly**, and **standard-monthly**.
   Supported regions are: **us-south**, **us-east**, **eu-gb**, **eu-de**, **jp-tok** and **au-syd**.

   Sample cluster specification JSON file:
   ```
   {
     "num_compute_nodes": 1,
     "hardware_config": "default",
     "software_package": "ae-1.2-hive-spark"
   }
   ```

### Description of the cluster specification parameters

The cluster parameters include:

1. **`num_compute_nodes`** (Required): Number of compute nodes in the cluster.
1. **`num_task_nodes`** (Optional): Number of task nodes in the cluster. You can create a cluster with no task nodes.
1. **`autoscale_policy`** (Optional): Defines the policy for automatic up or down scaling of task nodes in the cluster. See [Provisioning an auto scaling cluster](/docs/AnalyticsEngine?topic=AnalyticsEngine-autoscaling-clusters){: new_window}.
1. **`hardware_config`** (Required): Represents the instance size of the cluster. Accepted value: _`default`_ and _`memory-intensive`_  
1. **`software_package`** (Required): Determines the set of services to be installed on the cluster. Accepted value: _`ae-1.2-hive-llap`_, _` ae-1.2-hive-spark`_, _`ae-1.2-hadoop-spark`_
1. **`customization`** (Optional): Array of customization actions to be run on all nodes of the cluster once it is created. At the moment, only one customization action can be specified. The various types of customization actions that can be specified are discussed in detail in [Customizing clusters](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-cluster){: new_window}.
1. **`advanced_options`** (Optional): JSON object with nested JSON objects for various custom configurations for components installed with the cluster. Advantage here is that the custom configurations are baked during cluster creation time which means that the cluster is created based on the provided custom configurations. For details on how to create a cluster with `advanced_options`, see [Using advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options){: new_window}.
  \n

The following sample is the ibmcloud CLI response to the create instance command example previously shown:  \n

```
Creating service instance MyServiceInstance in resource group Default of account <account details>...
OK
Service instance MyServiceInstance was created.
Name              Location   State     Type              Tags   
MyServiceInstance us-south   inactive  service_instance
```
Service provisioning happens asynchronously. Successful response just means that the provisioning request has been accepted. Service provisioning is considered complete only when the associated cluster is created and is `active`.

You can also specify the type of Cloud service endpoints for your service instance. See [Provisioning an instance with {{site.data.keyword.Bluemix_short}} service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).
{: note}

## Provision tracking

For an overview of how a cluster is provisioned and how to check your cluster provisioning state, see [Track cluster provisioning](/docs/AnalyticsEngine?topic=AnalyticsEngine-track-provisioning){: new_window}.


## Querying for service provisioning status

To query the service provisioning status, enter the following command:
```
ibmcloud resource service-instance MyServiceInstance
```

{: codeblock}

Sample response:
```
Name: MyServiceInstance
Id: crn:v1:bluemix:public:ibmanalyticsengine:us-south:a/XXXXX::
Location:              us-south   
Service Name:          ibmanalyticsengine
Service Plan Name:     lite   
Resource Group Name:   default   
State:                 active   
Type:                  service_instance   
Tags:
```

The state indicates the service provisioning status. When provisioning is ongoing, it's `inactive`. When provisioning has completed, it will be `active`.
{: note}

## Creating a service instance using the Resource Controller REST API

To invoke the Resource Controller REST API to create a service instance, you need a resource group ID.

To get the resource group ID, log into the {{site.data.keyword.Bluemix_short}} CLI and run the following command:
```
ibmcloud resource groups
```
{: codeblock}

Sample result:
```
Retrieving all resource groups under account <Account details..>
OK
Name      ID      Default Group   State
Default   XXXXX   true            ACTIVE
```

You also need the `resource_plan_id` to create a service instance. The following resource plan IDs are supported:

- For Lite use: `7715aa8d-fb59-42e8-951e-5f1103d8285e`
- For Standard-Hourly use: `3175a5cf-61e3-4e79-aa2a-dff9a4e1f0ae`
- For Standard-Monthly use: `34594484-afda-40e6-b93b-341fbbaed242`

To create an instance via the Resource Controller REST API enter:
```
curl \
  --request POST \
  --url 'https://resource-controller.cloud.ibm.com/v2/resource_instances'   \
  --header 'accept: application/json'   \
  --header 'authorization: Bearer <IAM token>'   \
  --data @provision.json

cat provision.json
{
    "name": "MyServiceInstance",
    "resource_plan_id": "3175a5cf-61e3-4e79-aa2a-dff9a4e1f0ae",
    "resource_group": "XXXXX",
    "target": "us-south",
    "parameters": {
        "hardware_config": "default",
        "num_compute_nodes": "1",
        "software_package": "ae-1.2-hive-spark"
    },
    "tags": [
        "my-tag"
    ]  
}

```
{: codeblock}

For more information on the Resource Controller REST API for creating an instance, see [Create (provision) a new resource instance](/apidocs/resource-controller/resource-controller#create-resource-instance){: new_window}.

To get the IAM token, see [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token){: new_window}.


## Plan upgrading

You can only upgrade from a Lite plan to a Standard-Hourly plan. You can upgrade by using the {{site.data.keyword.iae_full_notm}} UI or by using the {{site.data.keyword.Bluemix_short}} CLI.

To upgrade a Lite plan by using the {{site.data.keyword.iae_short}} service dashboard in {{site.data.keyword.Bluemix_short}}:

1. Open your {{site.data.keyword.iae_full_notm}} service dashboard.
1. Select the **Plan** tab, select the Standard-Hourly plan and save your changes.

To upgrade using the {{site.data.keyword.Bluemix_short}} CLI enter the following command:

```
ibmcloud resource service-instance-update <your service instance name> --service-plan-id <resource_plan_id>
```
{: codeblock}


`resource_plan_id` is the `plan_id` of the target plan that you want to upgrade your service instance to.
