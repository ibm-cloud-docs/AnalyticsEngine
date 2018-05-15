---

copyright:
  years: 2017,2018
lastupdated: "2018-05-15"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving service credentials and service end points

The cluster credentials and various service end points that the cluster exposes are made available to you as `service keys`.

You can fetch the cluster credentials and the service end points by:
* [Using cf CLI](#obtaining-the-credentials-using-the-cf-cli)
* [Using cf REST API](#obtaining-the-credentials-using-the-cf-rest-api)

## Obtaining the credentials using the cf CLI

You need to create a service key for the {{site.data.keyword.iae_full_notm}} service instance to obtain the cluster credentials and service endpoints.

### Creating the service key

To create a service key, enter the following command:

```
cf create-service-key <your_service_instance_name> <your_service_key_name>
```
`<your_service_instance_name>` is the name of the service instance you  specified when creating the cluster. You can use `cf services` to find all your service instance names.

`<your_service_key_name>` is any name that you want to refer your key as. This name is used to retrieve service keys.  


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

`<service instance name>` is the the name of the service instance you  specified when creating the cluster.

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
      "ambari_console": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:9443",
      "notebook_gateway": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/jkg/",
      "notebook_gateway_websocket": "wss://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/jkgws/",
      "webhdfs": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/webhdfs/v1/",
      "ssh": "ssh clsadmin@XXXXX-mn003.bi.services.<changeme>.bluemix.net",
      "livy": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/livy/v1/batches"
    }
  },
  "cluster_management": {
    "api_url": "https://api.dataplatform.ibm.com/v2/analytics_engines/XXXXX",
    "instance_id": "XXXXX",
    "api_key": "XXXXX"
  }
}
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

In the sample response, the properties under `cluster` name the cluster user name, the password, and cluster service endpoints.


## Obtaining the credentials using the cf REST API

**Prerequisite**: You need the Cloud Foundry UAA bearer token. For more information, see [Obtaining the Cloud Foundry UAA access  token](./retrieving-uaa-access-token.html).

The API endpoint that handles API service keys is `https://api.ng.bluemix.net/v2/service_keys`.

### Creating the service key

To create a service key, enter:
```
curl -X POST \
  https://api.ng.bluemix.net/v2/service_keys \
  -H 'accept: application/json' \
  -H 'authorization: Bearer <User's UAA bearer token>' \
  -H 'content-type: application/json' \
  -d '{"name":"<key name>","service_instance_guid":"<service instance id>"}'
```
{:codeblock}

Sample response:
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
               "ambari_console": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:9443",
               "notebook_gateway": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/jkg/",
               "notebook_gateway_websocket": "wss://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/jkgws/",
               "webhdfs": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/webhdfs/v1/",
               "ssh": "ssh clsadmin@XXXXX-mn003.bi.services.<changeme>.bluemix.net",
               "livy": "https://XXXXX-mn001.bi.services.<changeme>.bluemix.net:8443/gateway/default/livy/v1/batches"
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
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.
