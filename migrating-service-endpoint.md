---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-01"

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

# Migrating from classic instances: get service endpoints
{: #get-service-endpoints}

This topic helps you when migrating from classic to serverless instances by providing a brief summary of the differences and sample code snippets for getting the service endpoints.


## Getting service endpoints

The service instance endpoint API calls are the same for classic and serverless instances as you can see from the following examples.

Classic instance endpoint API example:
```
-X POST https://resource-controller.cloud.ibm.com/v1/resource_keys  
-H 'accept: application/json'   
-H 'authorization: Bearer <IAM bearer token>' 
-H 'content-type: application/json' 
-d '{"name":"myservicecreds", "source_crn":"<service instance crn>", "parameters":{"role_crn":"<crn of access role>"} }'
```

Corresponding serverless instance endpoint API example:
```
curl 
-X POST https://resource-controller.cloud.ibm.com/v1/resource_keys
-H 'accept: application/json' 
-H 'authorization: Bearer <IAM bearer token>' 
-H 'content-type: application/json' 
-d '{"name":"myservicecreds", "source_crn":"<service instance crn>", "parameters":{"role_crn":"<crn of access role>"} }'
```

<br>

The following table summarizes the differences in the responses between classic and serverless instances when you invoke the API to get the service endpoints.


| Classic instance API response summary | Serverless instance API response summary |
|---------------------------------------|------------------------------------------|
| - You can use the cluster management user interface to add and delete nodes.  \n - Private service endpoints are accessible through Ambari, Livy and SSH, against which you can submit Spark jobs.  \n - `GUID` is the unique service instance ID, also used in the used in the cluster management APIs.  \n - `api-key` contains an IAM API key that can be used to generate IAM bearer tokens. An IAM bearer token must be provided for authorization when invoking the service management API URL. |  - You can manage your applications using the application API endpoint, for example `https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/cc44e03b-24df-495b-abbc-919e806ce127/spark_applications`   \n - You can manage your instance using the instance API endpoint, for example `https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/cc44e03b-24df-495b-abbc-919e806ce127`|

