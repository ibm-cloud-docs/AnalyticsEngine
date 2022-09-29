---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-29"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}

# Provisioning an {{site.data.keyword.iae_full_notm}} serverless instance
{: #provisioning-serverless}

You can create a serverless {{site.data.keyword.iae_full_notm}} service instance:

- [Using the {{site.data.keyword.Bluemix_notm}} console](#console-provisioning)
- [Using the {{site.data.keyword.Bluemix_notm}} command-line  interface](#cli-provisioning)
- [Using the Resource Controller REST API](#rest-api-provisioning)

Note that you are not able to define certain limitation and quota settings while provisioning a serverless instance. These values are predefined. See [Limits and quotas for {{site.data.keyword.iae_short}} instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits) for a list of these settings and their values.

You must have access to either the {{site.data.keyword.Bluemix_short}} `us-south` (Dallas) or the `eu-de` (Frankurt) region.
{: important}

## Creating a service instance from the IBM Cloud console
{: #console-provisioning}

You can create an instance using the {{site.data.keyword.Bluemix_notm}} console. To understand the concepts behind provisioning settings in the UI, see [Architecture and concepts in serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts).

To create an {{site.data.keyword.iae_full_notm}} instance:
1. Log into the [{{site.data.keyword.Bluemix_short}} console](https://{DomainName}/catalog){: external}.
1. Click **Sevices** and select the category **Analytics**.
1. Search for `{{site.data.keyword.iae_short}}` and then click on the tile to open the service instance creation page.
1. Choose the location in which you want the service instance to be deployed. Currently,  **us-south** and **eu-de** are the only supported regions.
1. Select a plan. Currently, **Standard Serverless for Apache Spark** is the only supported serverless plan.
1. Configure the instance by entering a name of your choice, selecting a resource group and adding tags.
1. Select the default Spark runtime. You can choose between Spark 3.1 and Spark 3.3  runtimes. The runtime pre-installs the spatio-temporal, data skipping and Paquet modular encryption packages by default.
1. Select the {{site.data.keyword.cos_full_notm}} instance from your account that you want to use as the `instance home` to store instance related data.
1. Add Spark configuration values to override default Apache Spark settings.
1. Click **Create** to provision the service instance in the background.

    The newly created service is listed in your [{{site.data.keyword.Bluemix_short}} resource list](https://{DomainName}/resources){: external} under **Services*.  

## Creating a service instance using the IBM Cloud command-line interface
{: #cli-provisioning}

To create a service instance using the {{site.data.keyword.Bluemix_short}} command-line interface:

1. Download and configure the {{site.data.keyword.Bluemix_short}} CLI. Follow the instructions in [Getting started with the {{site.data.keyword.Bluemix_short}} CLI](/docs/cli?topic=cli-getting-started).

1. Set the API endpoint for your region and log in:
    ```sh
    ibmcloud api https://{DomainName}
    ibmcloud login
    ```
    {: codeblock}

1. Get the list of the resource groups for your account and select one of the returned resource group as the target resource group in which to create the {{site.data.keyword.iae_full_notm}} serverless instance:
    ```sh
    ibmcloud resource groups
    ibmcloud target -g <resource_group_name>
    ```
    {: codeblock}

1. Create a service instance:
    ```sh
    ibmcloud resource service-instance-create <service_instance_name> ibmanalyticsengine <plan_name> <region> -p @<path_to JSON file with cluster parameters>
    ```
    {: codeblock}

    For example, for the Dallas region:
    ```sh
    ibmcloud resource service-instance-create MyServiceInstance ibmanalyticsengine standard-serverless-spark us-south -p @provision.json
    ```
    {: codeblock}

    You can give the service instance any name you choose. Note that currently, **standard-serverless-spark** is the only supported serverless plan and **us-south** and **eu-de** the only supported regions.

    The provision.json file contains the provisioning parameters for the instance you want to create.

    The endpoint to your {{site.data.keyword.cos_full_notm}} instance in the payload JSON file should be the public endpoint.

    <!--The endpoint to your {{site.data.keyword.cos_full_notm}} instance in the payload JSON file should be the `direct` endpoint. You can find the `direct` endpoint to your {{site.data.keyword.cos_full_notm}} instance on the {{site.data.keyword.Bluemix_short}} dashboard by selecting cross regional resiliency, the location, which should preferably match the location of your {{site.data.keyword.iae_short}} instance, and then clicking on your service instance. You can copy the direct endpoint from the **Endpoints** page.-->

    This is a sample of what the provision.json file can look like. See [Architecture and concepts in serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts) for a description of the provisioning parameters in the payload.

    Note that both Spark 3.1 and Spark 3.3 are supported. If you don't specify a default  Spark runtime version when you create a service instance, Spark 3.1 is taken by default.

    ```json
    {
      "default_runtime": {
        "spark_version": "3.1"
        },
      "instance_home": {
        "region": "us-south",
        "endpoint": "https://s3.us-south.cloud-object-storage.appdomain.cloud",
        "hmac_access_key": "<your-hmac-access-key",
        "hmac_secret_key": "<your-hmac-secret-key"
        },
      "default_config": {
        "key1": "value1",
        "key2": "value2"
        }
    }
    ```
    {: codeblock}

    The {{site.data.keyword.Bluemix_short}} response to the create instance command:
    ```text
    Creating service instance MyServiceInstance in resource group Default of account <your account name> as <your user name>...
    OK
    Service instance MyServiceInstance was created.
                            
    Name:                MyServiceInstance   
    ID:                  crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2cc7e4373bb0c9d2229f2ece5:1e32e***-afd9-483a-b1**-724ba5cf4***::   
    GUID:                1e32e***-afd9-483a-b1**-724ba5cf4***   
    Location:            us-south   
    State:               provisioning   
    Type:                service_instance   
    Sub Type:               
    Service Endpoints:   public   
    Allow Cleanup:       false   
    Locked:              false   
    Created at:          2021-11-29T07:20:40Z   
    Updated at:          2021-11-29T07:20:42Z   
    Last Operation:                      
                        Status    create in progress      
                        Message   Started create instance operation   
    ```

    Make a note of the instance ID from the output. You will need the instance ID when you call instance management or Spark application management APIs. See [Spark application REST API](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api).
    {: important}

1. [Track instance readiness](#instance-readiness).


## Creating a service instance using the Resource Controller REST API
{: #rest-api-provisioning}

An {{site.data.keyword.iae_full_notm}} serverless instance must reside in an {{site.data.keyword.Bluemix_short}} resource group. As a first step towards creating an {{site.data.keyword.iae_full_notm}} serverless instance through the Resource Controller REST API, you need to have the resource group ID and serverless plan ID close at hand.

To create a service instance using the Resource Controller REST API:

1. Get the resource group ID by logging into the {{site.data.keyword.Bluemix_short}} CLI and running the following command:
    ```sh
    ibmcloud resource groups
    ```
    {: codeblock}

    Sample result:
    ```text
    Retrieving all resource groups under account <Account details..>
    OK
    Name      ID      Default Group   State
    Default   XXXXX   true            ACTIVE
    ```
1. Use the following resource plan ID for the Standard Serverless for Apache Spark plan:
    ```text
    8afde05e-5fd8-4359-a597-946d8432dd45
    ```
    {: codeblock}

1. Get the IAM token by performing the following [steps](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Create an instance using the Resource Controller REST API:
    ```sh
    curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances/
    --header "Authorization: Bearer $token" -H 'Content-Type: application/json' -d @provision.json
    ```
    {: codeblock}

    The provision.json file contains the provisioning parameters for the instance you want to create. See [Architecture and concepts in serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts) for a description of the provisioning parameters in the payload.

    Note that both Spark 3.1 and Spark 3.3 are supported. If you don't specify a default  Spark runtime version when you create a service instance, Spark 3.1 is taken by default.

    This is a sample of what the provision.json file can look like:
    ```json
    {
      "name": "your-service-instance-name",
      "resource_plan_id": "8afde05e-5fd8-4359-a597-946d8432dd45",
      "resource_group": "resource-group-id",
      "target": "us-south",
      "parameters": {
        "default_runtime": {
          "spark_version": "3.1"
            },
            "instance_home": {
              "region": "us-south",
              "endpoint": "s3.us-south.cloud-object-storage.appdomain.cloud",
              "hmac_access_key": "your-access-key",
              "hmac_secret_key": "your-secret-key"
              }        
        }
    }
    ```
    {: codeblock}

1. [Track instance readiness](#instance-readiness).

For more information on the Resource Controller REST API for creating an instance, see [Create (provision) a new resource instance](/apidocs/resource-controller/resource-controller#create-resource-instance).

## Tracking instance readiness
{: #instance-readiness}

To run applications on a newly created serverless instance, the instance must be in `active` state.

To track instance readiness:
1. Enter the following command:
    ```sh
    curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/{instance_id} -H "Authorization: Bearer $token"
    ```
    {: codeblock}

    Sample response:
    ```json
    {
      "id": "dc0e****-eab2-4t9e-9441-56620949****",
      "state": "created",
      "state_change_time": "2021-04-21T04:24:01Z",
      "default_runtime": {
        "spark_version": "3.1",
        "instance_home": {
          "provider": "ibm-cos",
          "type": "objectstore",
          "region": "us-south",
          "endpoint": "https://s3.us-south.cloud-object-storage.appdomain.cloud",
          "bucket": "ae-bucket-do-not-delete-dc0e****-eab2-4t**-9441-566209499546",
          "hmac_access_key": "eH****g=",
          "hmac_secret_key": "4d********76"
        },
        "default_config": {
          "spark.driver.memory": "4g",
          "spark.driver.cores": 1
        }
      }
    }
    ```
1. Check the value of the `"state"` attribute. It must be `active` before you can start running applications in the instance.
