---

copyright:
  years: 2017, 2023
lastupdated: "2023-09-05"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving service endpoints
{: #retrieve-endpoints-serverless}

The service endpoints that the cluster exposes are made available to you as service keys (also known as service credentials).

You can fetch the service endpoints by:

- [Using the IBM Cloud CLI](#endpoints-cli)
- [Using the IBM Cloud REST API](#endpoints-api)
- [From the IBM Cloud console](#endpoints-console)

The service endpoints do not expose the instance credentials. To get the instance credentials, see [Retrieving cluster credentials](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-cluster-credentials).

## Obtaining the service endpoints using the IBM Cloud CLI
{: #endpoints-cli}

You need to create a service key for the {{site.data.keyword.iae_full_notm}} serverless instance to obtain the service endpoints.

To create a service resource key, enter the following command:
```sh
ibmcloud resource service-key-create <your_service_key_name> <role> --instance-name <your_service_instance_name>
```
{: codeblock}

Required parameters include:

-	`<your_service_instance_name>`: the name of the service instance you specified when creating the cluster. You can check your [IBM Cloud resource list](https://cloud.ibm.com/resources) to find your service instance names.
-	`<your_service_key_name>`: any name that you want to use for the key. This name is used to retrieve service keys.
- `<role>`: the role you assigned to the IAM API key which was generated for the service credentials. You will be able to perform only those operations that are permitted for the chosen role. For more details on the roles required to perform an operation, refer to section *Required IAM permissions* in [Granting permissions to users](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).


Expected response:
```text
Creating service key of service instance <service_instance_name> under account <your account name> as <your user name>...
OK
Service key <service key crn> was created.
Name:          service-key-name
ID:            crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2cxxxxx229f2xxxx:1671b660-xxxx-448f-xxx-e1633a2b97c7:resource-key:4f1b043c-xxxx-xxxx-xxxx-cbf90f6155d0
Created At:    Mon Nov 29 11:47:32 UTC 2021
State:         active
Credentials:
               apikey:                   xxxxxx
               endpoints:
                                         applications_api:   https://api-staging.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx660-7f2x-4xxf-9xx4-exxxxxxxxxc7/spark_applications
                                         instance_api:       https://api-staging.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx660-7f2x-4xxf-9xx4-exxxxxxxxxc7
					 history_server_ui:  https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx660-7f2x-4xxf-9xx4-exxxxxxxxxc7/spark_history_ui
					 history_server_api: https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx660-7f2x-4xxf-9xx4-exxxxxxxxxc7/spark_history_api/v1

               iam_apikey_description:   Auto-generated for key 4f1xxx3c-7517-xxx-cbfxxxxxx5d0
               iam_apikey_name:          service-key-name
               iam_role_crn:             crn:v1:bluemix:public:iam::::serviceRole:Writer
               iam_serviceid_crn:        crn:v1:staging:public:iam-identity::a/xxxx::serviceid:ServiceId-3543db7f-xxxx-454a-xxxx-1b20333ebeaa
```

To view your service resource key, enter the following command:
```sh
ibmcloud resource service-key <service_key_name>
```
{: codeblock}

Required parameter:

- `<service key name>`: the name of the service key that you entered when creating the key.

Sample Response: Bear in mind that the cluster credentials are not returned in the response.
```json
{
  "apikey": "xxxxx",
  "endpoints": {
    "applications_api": "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx-3bxxxbc-4xxx1-axx6-8xxdxx0xxd/spark_applications",
    "instance_api": "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx-3bxxxbc-4xxx1-axx6-8xxdxx0xxd",
    "history_server_ui": "https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx-3bxxxbc-4xxx1-axx6-8xxdxx0xxd/spark_history_ui",
    "history_server_api":"https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/xxxx-3bxxxbc-4xxx1-axx6-8xxdxx0xxd/spark_history_api/v1"
  },
  "iam_apikey_description": "Auto-generated for key cabde209-xxxx",
  "iam_apikey_name": "Service credentials-1",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Reader",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/xxxxx::serviceid:ServiceId-2fec11aa-xxxx-4918-xxxx-aa3650f99050"
}
```

In the sample response, the properties under `endpoints` specify the service endpoints. The property `iam_apikey_name` contains an IAM API key that can be used to generate IAM bearer tokens. An IAM bearer token must be provided for authorization when invoking the REST APIs for  {{site.data.keyword.iae_full_notm}} serverless instances.


## Obtaining the service endpoints using the IBM Cloud REST API
{: #endpoints-api}

**Prerequisite**: You need an IAM bearer token. For more information, see [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).

The API endpoint that handles API service keys is `https://resource-controller.cloud.ibm.com/v1/resource_keys`.

To create a service resource key, enter:
```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v1/resource_keys \
  -H 'accept: application/json' \
  -H "authorization: Bearer <IAM bearer token>" \
  -H 'content-type: application/json' \
  -d '{"name":"<key name>","source_crn":"<service instance crn>", "parameters":{"role_crn":"<crn of access role>"} }'
```
{: codeblock}

Sample response:
```json
{
	"id": "crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2cxxxxx229f2xxxx:79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx:resource-key:2278xxxx-xxxx-xxxx-xxxx-59a6da6xxxxe",
	"guid": "2278xxxx-xxxx-xxxx-xxxx-59a6da68xxxx",
	"url": "/v1/resource_keys/crn%3Av1%3Astaging%3Apublic%3Aibmanalyticsengine%3Aus-south%3Aa%2Fd628eae2cxxxxx229f2xxxx%3A79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx%3Aresource-key%3A2278xxxx-xxxx-xxxx-xxxx-59a6da6xxxxe",
	"created_at": "2021-11-30T08:48:47.909145179Z",
	"updated_at": "2021-11-30T08:48:47.909145179Z",
	"deleted_at": null,
	"created_by": "",
	"updated_by": "",
	"deleted_by": "",
	"source_crn": "crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2cxxxxx229f2xxxx:79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx::",
	"name": "service_key_02",
	"role": "crn:v1:bluemix:public:iam::::serviceRole:Writer",
	"crn": "crn:v1:staging:public:ibmanalyticsengine:us-south:a/d628eae2cxxxxx229f2xxxx:79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx:resource-key:2278xxxx-xxxx-xxxx-xxxx-59a6da6xxxxe",
	"state": "active",
	"account_id": "d628eae2cxxxxx229f2xxxx",
	"resource_group_id": "65828fxxxx594594816e872xxxx",
	"resource_id": "18dexxxx-xxxx-xxxx-xxxx-0ed2f93fxxxx",
	"credentials": {
		"apikey": "xxxxx",
		"endpoints": {
			"applications_api": "https://api-staging.us-south.ae.cloud.ibm.com/v3/analytics_engines/79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx/spark_applications",
			"instance_api": "https://api-staging.us-south.ae.cloud.ibm.com/v3/analytics_engines/79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx",
			"history_server_ui": "https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx/spark_history_ui",
			"history_server_api":"https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx/spark_history_api/v1"
		},
		"iam_apikey_description": "Auto-generated for key 2278e09d-b815-4c4b-b824-xxxx",
		"iam_apikey_name": "service_key_02",
		"iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer",
		"iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/d628eae2cxxxxx229f2xxxx::serviceid:ServiceId-209exxxx-xxxx-xxxx-xxxx-4ab9ce16xxxx"
	},
	"iam_compatible": true,
	"migrated": false,
	"resource_instance_url": "/v1/resource_instances/crn%3Av1%3Astaging%3Apublic%3Aibmanalyticsengine%3Aus-south%3Aa%2Fd628eae2cxxxxx229f2xxxx%3A79c757e9-xxxx-xxxx-xxxx-dff7f01xxxx%3A%3A",
	"resource_alias_url": null
}
```

## Obtaining the service endpoints from the IBM Cloud console
{: #endpoints-console}

If you follow the steps in this section to get the service endpoints using the IBM Cloud console, you are directed to the service credentials page for your service instance where you expect to see service endpoints and API key.

To create a service key from the IBM Cloud console:
1. Go to your [resource list](https://cloud.ibm.com/resources), click **Services and software** and select the provisioned serverless instance.
1. Click **Service credentials** in the left side bar.
1. Then click **New credential** to create new service credentials.
1. Enter a name, select a role, and click **Add**.
1. Copy the credentials to the clipboard.

See the [API documentation](/apidocs/ibm-analytics-engine/ibm-analytics-engine-v3) for the operations that are available on the instance management and application management endpoints. For details on the permissions that are required to invoke operations on those endpoints, see [Granting permissions to users](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
