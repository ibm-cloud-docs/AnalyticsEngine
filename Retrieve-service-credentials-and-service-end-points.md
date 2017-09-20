---

<<<<<<< HEAD
=======
copyright:
  years: 2017
lastupdated: "2017-09-12"

---

>>>>>>> refs/remotes/Bluemix-Docs/staging
<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving service credentials and service end points

The cluster credentials and various service end points that the cluster exposes are made available to you as `service keys`. As described below, you need to create a `service key` on the Cloud Foundry service instance you [provisioned](./provisioning.html#how-to-provision-a-service-instance).

<<<<<<< HEAD
You can fetch the cluster credentials and the service end points by:
* [Using CF CLI](#obtaining-credentials-using-cfcli)
* [Using CF REST API](#obtaining-credentials-using-cloud-foundry-rest-apis)

## Obtaining credentials using cf CLI

You need to create a service key for the Analytics Engine service instance to obtain the cluster credentials and service endpoints.

### Creating a service key
To create a service key, enter the following command: 
 
```
cf create-service-key <your_service_instance_name> <your_service_key_name>
```
`<your_service_instance_name>` is the name of the service instance you  specified when creating the cluster.

`<your_service_key_name>` is any name that you want to refer your key as. This name is used to retrieve service keys.  
=======
The cluster credentials and various service end points that the cluster exposes are made available to you as `service keys`. As described below, you need to create a `service key` on the cloudfoundry service instance that you [provisioned](./provisioning.html#how-to-provision-a-service-instance).

You can fetch the cluster credentials and the service end points through one of the following ways:
* [Using cf CLI](#fetch-credentials-using-the-cf-cli)
* [Using cf REST API](#fetch-credentials-using-the-cf-rest-api)

## Fetch credentials using the cf CLI
Cluster credentials and service endpoints by creating and viewing a service key for the IBM Analytics Engine service instance created by the user.

### Creating a service key
```
cf create-service-key <your_service_instance_name> <your_service_key_name>
```
For `<your_service_instance_name>`, specify the name of the service you had specified when creating the cluster.

You can use `cf services` to find all your service instance names.
>>>>>>> refs/remotes/Bluemix-Docs/staging

Expected response:

```
Creating service key <service_key_name> for service instance <service_instance_name> as user...
OK
```

### Viewing the service key
To view your service key, enter the following command:

```
cf service-key <service_instance_name> <service_key_name>
```
<<<<<<< HEAD

`<service instance name>` is the the name of the service instance you  specified when creating the cluster.
=======
For `<service_instance_name>`, specify the name of the service instance you had specified when creating the cluster.

For `<service_key_name>`, specify the name of the service key that you entered when creating the key.
>>>>>>> refs/remotes/Bluemix-Docs/staging

`<service key name>` is the name of the service key that you entered when creating the key.

Sample Response:

```
{
  "cluster": {
    "cluster_id": "XXXXX",
    "user": "clsadmin",
    "password": "XXXXX",
    "password_expiry_date": "null",
    "service_endpoints": {
      "ambari_console": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:9443",
      "notebook_gateway": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/",
      "notebook_gateway_websocket": "wss://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/",
      "webhdfs": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/webhdfs/v1/",
      "ssh": "ssh clsadmin@XXXXX-mn003.bi.services.us-south.bluemix.net",
      "livy": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
    }
  },
  "cluster_management": {
    "api_url": "https://api.dataplatform.ibm.com/v2/analytics_engines/XXXXX",
    "instance_id": "XXXXX",
    "api_key": "XXXXX"
  }
}
```
In the sample response, the properties under `cluster` name the cluster user name, the password, and cluster service endpoints.


### Obtaining credentials using Cloud Foundry REST APIs

The API endpoint that handles API service keys is `https://api.ng.bluemix.net/v2/service_keys`.

<<<<<<< HEAD
Enter the following API to creating a service key:

=======
### Fetch credentials using the cf REST API

**Pre-requisite**: You need the Cloud Foundry UAA bearer token. For more information, see [Obtaining the Cloud Foundry UAA bearer token](./provisioning.html#Obtaining-the-Cloud-Foundry-UAA-bearer-token).

URL: `https://api.ng.bluemix.net/v2/service_keys`<br>
Creating a service key:
>>>>>>> refs/remotes/Bluemix-Docs/staging
```
curl -X POST \
  https://api.ng.bluemix.net/v2/service_keys \
  -H 'accept: application/json' \
  -H 'authorization: Bearer <User's UAA bearer token>' \
  -H 'content-type: application/json' \
  -d '{"name":"<key name>","service_instance_guid":"<service instance id>"}'
```
<<<<<<< HEAD
{:codeblock}

Sample response:


=======
Sample Response:
>>>>>>> refs/remotes/Bluemix-Docs/staging
```
{
  "metadata": {
    "guid": "855f1b10-96bb-401a-886d-44511d76cf66",
    "url": "/v2/service_keys/855f1b10-96bb-401a-886d-44511d76cf66",
    "created_at": "2017-04-13T06:59:26Z",
    "updated_at": null
  },
  "entity": {
    "name": "mykey4",
    "service_instance_guid": "7e710fcf-9744-4ad0-9896-131aa8a3c99e",
    "credentials": {
      "cluster": {
         "cluster_id": "XXXXX",
         "user": "clsadmin",
         "password": "XXXXX",
         "password_expiry_date": "null",
         "service_endpoints": {
               "ambari_console": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:9443",
               "notebook_gateway": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/",
               "notebook_gateway_websocket": "wss://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/",
               "webhdfs": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/webhdfs/v1/",
               "ssh": "ssh clsadmin@XXXXX-mn003.bi.services.us-south.bluemix.net",
               "livy": "https://XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
         }
      },
     "cluster_management": {
         "api_url": "https://api.dataplatform.ibm.com/v2/analytics_engines/XXXXX",
         "instance_id": "XXXXX",
         "api_key": "XXXXX"
      }
    },
    "service_instance_url": "/v2/service_instances/XXXXX"
  }
}

```