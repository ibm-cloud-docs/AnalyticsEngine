---

copyright:
  years: 2017
lastupdated: "2017-08-04"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieve service credentials and service end points

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

For `<your_service_key_name>`, specify any name that you want to refer your key as. This can be used later to retrieve service keys.

Expected Response:
```
Creating service key <service_key_name> for service instance <service_instance_name> as user...
OK
```

*Viewing the service key:*
```
cf service-key <service_instance_name> <service_key_name>
```
For `<service_instance_name>`, specify the name of the service instance you had specified when creating the cluster.

For `<service_key_name>`, specify the name of the service key that you entered when creating the key.

### Sample Response

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
    "api_url": "https://ibmae-api.mybluemix.net/v2/analytics_engines/XXXXX",
    "instance_id": "XXXXX",
    "api_key": "XXXXX"
  }
}
```
In the sample response above, properties under `cluster` give the cluster user name, password and cluster service endpoints.

***

### Fetch credentials using the cf REST API

URL: `https://api.ng.bluemix.net/v2/service_keys`<br>
Creating a service key:
```
curl -X POST \
  https://api.ng.bluemix.net/v2/service_keys \
  -H 'accept: application/json' \
  -H 'authorization: <User's bearer token>' \
  -H 'content-type: application/json' \
  -d '{"name":"<key name>","service_instance_guid":"<service instance id>"}'
```
Sample Response:
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
         "api_url": "https://ibmae-api.ng.bluemix.net/v2/analytics_engines/XXXXX",
         "instance_id": "XXXXX",
         "api_key": "XXXXX"
      }
    },
    "service_instance_url": "/v2/service_instances/XXXXX"
  }
}

```
