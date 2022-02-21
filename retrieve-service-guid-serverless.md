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
    ```sh
    $ ibmcloud resource service-instances --service-name ibmanalyticsengine
    ```
    {: codeblock}

    This call retrieves the instances of type `service_instance` in all resource groups in all locations for your account.

    Example response:
    ```text
    Name                  Location   State    Type               Resource Group ID   
    serverless-instance   us-south   active   service_instance   65xxxxxxxxxxxxxxxa3fd 
    ```
1. Enter the following command with the server instance name of your instance to view the instance details:
    ```sh
    $ ibmcloud resource service-instance "Analytics Engine-xyz"
    ```
    {: codeblock}

    This retrieves your instance from the resource groups under your account.

    Example response:
    ```text                   
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
    ```sh
    GET <resource-controller-url>/v2/resource_instances
    ```
	{: codeblock}

    Example of a request:
    ```sh
    curl -X GET https://resource-controller.cloud.ibm.com/v2/resource_instances? resource_plan_id=8afxxxx-xxxx-xxxx-xxxx-946d843xxxx -H "Authorization: Bearer <>" \
    ```
	{: codeblock}
	
    Example response:
    ```json
    {
	"rows_count": 1,
	"next_url": null,
	"resources": [{
		"id": "crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2ccxxxx3bb0c9dxxxx:da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx::",
		"guid": "da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx",
		"url": "/v2/resource_instances/da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx",
		"created_at": "2021-08-05T11:05:51.545526066Z",
		"updated_at": "2021-11-04T05:11:30.966202521Z",
		"deleted_at": null,
		"created_by": "IBMid-661002042N",
		"updated_by": "",
		"deleted_by": "",
		"scheduled_reclaim_at": null,
		"restored_at": null,
		"scheduled_reclaim_by": "",
		"restored_by": "",
		"name": "serverless-instance",
		"region_id": "us-south",
		"account_id": "d628eae2ccxxxx3bb0c9dxxxx",
		"reseller_channel_id": "",
		"resource_plan_id": "8afxxxx-xxxx-xxxx-xxxx-946d843xxxx",
		"resource_group_id": "65828fxxxx594594816exxx",
		"resource_group_crn": "crn:v1:staging:public:resource-controller::a/d628eae2ccxxxx3bb0c9dxxxx::resource-group:65828fxxxx594594816exxx",
		"target_crn": "crn:v1:staging:public:globalcatalog::::deployment:8afxxxx-xxxx-xxxx-xxxx-946d843xxxx%3Aus-south",
		"parameters": {
			"default_config": {
				"spark.driver.cores": "1",
				"spark.driver.memory": "512m",
				"spark.executor.cores": "1",
				"spark.executor.instances": "1"
			},
			"default_runtime": {
				"additional_packages": ["parquet-modular-encryption"],
				"spark_version": "3.0.0"
			},
			"instance_home": {
				"endpoint": "s3.direct.us-south.cloud-object-storage.appdomain.cloud",
				"guid": "902xxxx-xxxx-xxxx-xxxx-4127da8xxxx",
				"hmac_access_key": "xxxxxxxxx",
				"hmac_secret_key": "xxxxxxxx",
				"provider": "ibm-cos",
				"region": "us-south",
				"type": "objectstore"
			},
			"service-endpoints": "public"
		},
		"allow_cleanup": false,
		"crn": "crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2ccxxxx3bb0c9dxxxx:da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx::",
		"state": "active",
		"type": "service_instance",
		"resource_id": "18dexxxx-xxxx-xxxx-xxxx-0ed2f9xxxx",
		"dashboard_url": null,
		"last_operation": {
			"type": "delete",
			"state": "failed",
			"async": false,
			"description": "[500, Internal Server Error] The request could not be processed. Try again later."
		},
		"resource_aliases_url": "/v2/resource_instances/da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx/resource_aliases",
		"resource_bindings_url": "/v2/resource_instances/da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx/resource_bindings",
		"resource_keys_url": "/v2/resource_instances/da82xxxx-xxxx-xxxx-xxxx-d0faf90exxxx/resource_keys",
		"plan_history": [{
			"resource_plan_id": "8afxxxx-xxxx-xxxx-xxxx-946d843xxxx",
			"start_date": "2021-08-05T11:05:51.545526066Z",
			"requestor_id": "IBMid-661002042N"
		}],
		"migrated": false,
		"controlled_by": "",
		"locked": false
	}]}
    ```
    See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless) for how to obtain the authorization bearer access token.

    Search for your instance name in the `"resources"` array in the response by checking the `"name"` parameter. The response includes the GUID and the provisioning state of your instance. For more details about the response of the API, see [Get a list of all resource instances](/apidocs/resource-controller/resource-controller#list-resource-instances).

    Note that the returned state `create succeeded` indicates that the provision request was successfully accepted. However, to run applications, the instance needs to move to `active` state. Track  the instance readiness before performing any operations on the instance. See [Tracking instance readiness](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless#instance-readiness).
