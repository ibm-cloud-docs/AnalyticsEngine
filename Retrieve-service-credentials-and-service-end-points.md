---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-24"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving service endpoints
{: #retrieve-endpoints}

The service endpoints that the cluster exposes are made available to you as `service keys` (aka service credentials).

You can fetch the service endpoints by:
* [Using the {{site.data.keyword.Bluemix_notm}} CLI](#obtaining-the-service-endpoints-using-the-ibm-cloud-cli)
* [Using the {{site.data.keyword.Bluemix_notm}} REST API](#obtaining-the-service-endpoints-using-the-ibm-cloud-rest-api)
* [From the {{site.data.keyword.Bluemix_notm}} console](#obtaining-the-service-endpoints-from-the-ibm-cloud-console)

The service endpoints do not expose the cluster credentials. To get the cluster credentials, see [Retrieving cluster credentials](/docs/servicµes/AnalyticsEngine?topic=AnalyticsEngine-retrieve-cluster-credentials).

## Obtaining the service endpoints using the {{site.data.keyword.Bluemix_notm}} CLI

You need to create a service key for the {{site.data.keyword.iae_full_notm}} service instance to obtain the service endpoints.

To create a service key, enter the following command:

```
ibmcloud resource service-key-create <your_service_key_name> <role> --instance-name <your_service_instance_name>
```
where:
- `<your_service_instance_name>` is the name of the service instance you  specified when creating the cluster. You can use `ibmcloud resource service-instances` to find all your service instance names.
- `<your_service_key_name>` is any name that you want to refer your key as. This name is used to retrieve service keys.  
- `<role>` is the role you assigned to the IAM API key which was generated for the service credentials. You will be able to perform only those operations that are permitted for the chosen role. For more details on the roles required to perform an operation, refer to section *Required IAM permissions* [here](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).

Expected response:

```
Creating service key <service_key_name> in resource group default of account <your account name> as <your user name>...
OK
Service key <service key crn> was created.
<service key value ….>

```

You can also view your service key by using the following command:

```
ibmcloud resource service-key <service_key_name>
```
where:

- `<service key name>` is the name of the service key that you entered when creating the key.

Sample Response:
Bear in mind that the cluster credentials are not returned in the response.

```
{
  "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - <instance crn>",
  "iam_apikey_name": "<api key name>",
  "iam_role_crn": "<crn for assigned iam role>",
  "iam_serviceid_crn": "<crn for iam service id associated with current api key>",
  "apikey": "<iam api key value>",
  "cluster": {
    "cluster_id": "xyz-xyz-xyz",
    "service_endpoints": {
      "ambari_console": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:9443",
      "livy": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches",
      "notebook_gateway": "https://xxxxx-102-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/jkg/",
      "notebook_gateway_websocket": "wss://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/jkgws/",
      "spark_history_server": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/sparkhistory",
      "ssh": "ssh xxxxxxx@xxxxx-mn003.<region>.ae.appdomain.cloud",
        "webhdfs": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/webhdfs/v1/"
      },
      "service_endpoints_ip": {
        "ambari_console": "https://xxx.xxx.xxx.xxx:9443",
        "livy": "https:// xxx.xxx.xxx.xxx:8443/gateway/default/livy/v1/batches",
        "notebook_gateway": "https://1 xxx.xxx.xxx.xxx:8443/gateway/default/jkg/",
        "notebook_gateway_websocket": "wss://xxx.xxx.xxx.xxx:8443/gateway/default/jkgws/",
        "spark_history_server": "https://xxx.xxx.xxx.xxx:8443/gateway/default/sparkhistory",
        "ssh": "ssh xxxxxx@xxx.xxx.xxx.xxx ",
        "webhdfs": "https:// xxx.xxx.xxx.xxx:8443/gateway/default/webhdfs/v1/"
      }

  },
  "cluster_management": {
    "api_url": "https://api.<region>.ae.cloud.ibm.com/v2/analytics_engines/f2bda953-90c0-4e9b-ab5f-7aa375193145",
    "instance_id": "xxxxxxxxxxxxxxx"
  }
}
```

where `<region>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

In the sample response, the properties under `cluster` specify the  cluster service endpoints. No cluster credentials are returned.

The property `apikey` contains an IAM API key that can be used to generate IAM bearer tokens. An IAM bearer token must be provided for authorization when invoking the cluster management API URL.

## Obtaining the service endpoints using the {{site.data.keyword.Bluemix_notm}} REST API

**Prerequisite**: You need an IAM bearer token. For more information, see [Retrieving IAM access tokens](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).

The API endpoint that handles API service keys is `https://resource-controller.bluemix.net/v1/resource_keys`.


To create a resource key, enter:
```
curl -X POST \
  https://resource-controller.bluemix.net/v1/resource_keys \
  -H 'accept: application/json' \
  -H 'authorization: Bearer <IAM bearer token>' \
  -H 'content-type: application/json' \
  -d '{"name":"<key name>","source_crn":"<service instance crn>", "parameters":{"role_crn":"<crn of access role>"} }'
```
{:codeblock}

Sample response:
```
{
  "resource_group_id": "43c5c7978b0644f9bd2890fea1fdeadf",
  "deleted_at": null,
  "migrated": false,
  "name": "aekey2xxxxxxxxx",
  "resource_id": "f6f931f9-f1ab-4fde-a0df-66094c2ddf62xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "url": "/v1/resource_keys/<service key crn>",
  "resource_instance_url": "/v1/resource_instances/<service instance crn>",
  "created_at": "2018-05-31T18:58:26.828296982Z",
  "updated_at": null,
  "source_crn": "<service instance crn>",
  "resource_alias_url": null,
  "state": "active",
  "credentials": {
    "apikey": "<iam api key>",
    "iam_role_crn": "<iam access role crn>",
    "cluster": {
      "cluster_id": "xxx-xxx-xxx-xxx",
      "service_endpoints_ip": {
      "spark_history_server": "https://xxx.xxx.xxx.xxx:8443/gateway/default/sparkhistory",
      "notebook_gateway": "https://xxx.xxx.xxx.xxx:8443/gateway/default/jkg/",
      "livy": "https://xxx.xxx.xxx.xxx:8443/gateway/default/livy/v1/batches",
      "webhdfs": "https://xxx.xxx.xxx.xxx:8443/gateway/default/webhdfs/v1/",
      "ambari_console": "https://xxx.xxx.xxx.xxx:9443",
      "notebook_gateway_websocket": "wss:// xxx.xxx.xxx.xxx:8443/gateway/default/jkgws/",
      "ssh": "ssh xxxxxxx@xxx.xxx.xxx.xxx "
      },
    "service_endpoints": {
      "spark_history_server": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/sparkhistory",
      "notebook_gateway": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/jkg/",
      "livy": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches",
      "webhdfs": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/webhdfs/v1/",
      "ambari_console": "https://xxxxx-mn001.<region>.ae.appdomain.cloud:9443",
      "notebook_gateway_websocket": "wss://xxxxx-mn001.<region>.ae.appdomain.cloud:8443/gateway/default/jkgws/",
      "ssh": "ssh xxxxxx@xxxxx-mn003.<region>.ae.appdomain.cloud"
      }
    },
    "cluster_management": {
      "instance_id": "xxxx-xxxx-xxxx-xxxx",
      "api_url": "https://api.<region>.ae.cloud.ibm.com/v2/analytics_engines/xxxx-xxxx-xxxx-xxxx"
      },
    "iam_apikey_name": "auto-generated-apikey-c1fb87dc-e37b-4da0-a486-69dece62cfcf",
    "iam_serviceid_crn": "crn of service id associated with api key",
    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - <service instance crn>"
  },
  "iam_compatible": true,
  "guid": "xxxx-xxxx-xxxx-xxxx-xxxx",
  "crn": "<crn of service key>",
  "id": "<crn of service key>",
  "account_id": "<user’s account id>"
}
```
where `<region>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`. No cluster credentials are returned.

## Obtaining the service endpoints from the {{site.data.keyword.Bluemix_notm}} console

If you follow the steps in this section to get the service endpoints by using the {{site.data.keyword.Bluemix_notm}} console, you are directed to the service credentials page for your service instance where you expect to see user credentials. You will notice however that the cluster user name and password are not exposed on this page, only the service endpoints.     

To create a service key from the {{site.data.keyword.Bluemix_notm}} console:
1. Select the provisioned service instance.
2. Click **Service credentials** in the left side bar.
3. Then click **New credential** to create a new service credential.
4. Enter a name, add configuration parameters (if any) and click **Add**.

The newly created endpoints are listed on this page. Click **View Credentials** to see the details.
