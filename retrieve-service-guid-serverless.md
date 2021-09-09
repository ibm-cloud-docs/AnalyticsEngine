---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-25"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving details of a serverless instance
{: #retrieve-instance-details}

You can retrieve information, like the instance ID (or GUID) and the provisioning state of an {{site.data.keyword.iae_full_notm}} serverless instance from the instance details. You need the instance ID to use the Spark application REST API and the Livy batch APIs.

You can retrieve the details by:

- [Using the {{site.data.keyword.Bluemix_notm}} CLI](#retrieve-guid-cli)
- [Using the {{site.data.keyword.Bluemix_notm}} REST API](#retrieve-guid-api)

## Accessing instance details by using the {{site.data.keyword.Bluemix_notm}} CLI
{: #retrieve-guid-cli}

To get the details of an instance:

1. List all of the created serverless instances:
    ```
    $ ibmcloud resource service-instances --service-name ibmanalyticsengine
    ```

    This call retrieves the instances of type `service_instance` in all resource groups in all locations for your account.

    Example response:
    ```
    Name                           Location   State    Type     
    Analytics Engine-xyz           us-south   active   service_instance  
    ```
1. Enter the following command with the server instance name of your instance to view the instance details:
    ```
    $ ibmcloud resource service-instance "Analytics Engine-xyz"
    ```

    This retrieves your instance from the resource groups under your account.

    Example response:
    ```                         
    Name:                  Analytics Engine-xyz
    ID:
    crn:v1:staging:public:ibmanalyticsengine:us-south:a/XXXXX:XXXXX::
    GUID:                  XXXXX   
    Location:              us-south   
    Service Name:          ibmanalyticsengine   
    Service Plan Name:     standard-serverless-spark   
    Resource Group Name:   Default   
    State:                 active   
    Type:                  service_instance   
    Sub Type:                 
    Created at:            2021-01-06T07:49:12Z   
    Created by:            XXXXX   
    Updated at:            2021-01-06T07:51:01Z   
    Last Operation:                        
                           status    create succeeded      
                           Message   Started create instance operation
    ```

    The response includes the GUID and the provisioning state of your instance.

    Note that the returned state `create succeeded` indicates that the provision request was successfully accepted. However, in order to run applications, the instance needs to move to `active` state. Track the status of the instance readiness before performing any operation on the instance. See [Tracking instance readiness](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless#instance-readiness).

## Accessing instance details by using the {{site.data.keyword.Bluemix_notm}} REST API
{: #retrieve-guid-api}

You need the ID of a {{site.data.keyword.iae_full_notm}} serverless instance to get the details of the instance, which include the GUID and the provisioning state of the instance for example.

To get the details of an instance:

1. List all of the created serverless instances in the resource group your account:
    ```
    GET <resource-controller-url>/v2/resource_instances
    ```

    Example of a request:
    ```
    curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances? resource_plan_id=8afde05e-5fd8-4359-a597-946d8432dd45 -H 'Authorization: Bearer <>' \
    ```

    See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless) for how to obtain the authorization bearer access token.

    Search for your instance name in the `"resources"` array in the response by checking the `"name"` parameter. The response includes the GUID and the provisioning state of your instance. For more details about the response of the API, see [Get a list of all resource instances](/apidocs/resource-controller/resource-controller#list-resource-instances).

    Note that the returned state `create succeeded` indicates that the provision request was successfully accepted. However, to run applications, the instance needs to move to `active` state. Track  the instance readiness before performing any operations on the instance. See [Tracking instance readiness](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless#instance-readiness).
