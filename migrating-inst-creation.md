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

# Migrating from classic instances: instance creation
{: #instance-creation}

This topic helps you when migrating from classic to serverless instances by providing a brief summary of the differences and sample code snippets for creating instances using the REST API.

## Creating an instance using the REST API 

The following table summarizes the differences between classic and serverless instances when you use the create instance API.

| Operation | Classic instances | Serverless instances |
|-----------|-----------------------|--------------------------|
| Create instance API | Same command but changes to payload:  \n - Resource plan ID: `3175a5cf-61e3-4e79-aa2a-dff9a4e1f0ae`  \n - `"parameters"` element in payload defines the `{"hardware_config" + "num_compute_nodes" + "software_package" + "advanced_options(optional)" }` | Same command but changes to payload :  \n - Resource plan ID: `8afde05e-5fd8-4359-a597-946d8432dd45`  \n - `"parameters"` element in payload defines the `{"default_runtime" + "instance_home" + "default_config(optional)" }`|
| Track API summary | Uses the v2 API | Uses the v3 API |
| Create instance API with advanced options | Uses the `"advanced_options"` element in payload | Uses the `"default_config"` element in the payload |


## Sample create instance API calls

When migrating to the serverless instances, you need to change your create instance API call. This is an example of what your classic create instance call could look like:

```
curl 
--request POST 
--url "https://resource-controller.cloud.ibm.com/v2/resource_instances" 
--header 'accept: application/json' 
--header 'authorization: Bearer <IAM token>' 
--data @provision.json
```

Sample payload classic create instance call:
```
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

The equivalent to create a serverless instance would be:
```
curl 
--request POST 
"https://resource-controller.cloud.ibm.com/v2/resource_instances/" 
--header 'Content-Type: application/json' 
--header "Authorization: Bearer <IAM token>" 
--data @provision.json
```

The sample payload for serverless create instance call:
```
cat provision.json
{
  "name": "MyServiceInstance,
  "resource_plan_id": "8afde05e-5fd8-4359-a597-946d8432dd45",
  "resource_group": "XXXXX",
  "target": "us-south",
  "parameters": {
    "default_runtime": {
    "spark_version": "3.1"
    },
  "instance_home": {
    "region": "us-south",
    "endpoint": MMMM",
    "hmac_access_key": "your-access-key",
    "hmac_secret_key": "your-secret-key"
    }        
  }
}
```


## Track instance APIs

If you want to track the state of a provisioned instance when migrating to the serverless instances, you need to change the following v2 classic instance API call:

```
curl -i -X GET   https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<instance_id>/state -H 'Authorization: Bearer $token>'
```

to the following v3 serverless instance API call:

```
curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/{instance_id} -H "Authorization: Bearer $token"
```

## Create instance API with advanced Spark configuration

If you include advanced options in the create instance API when creating a classic service instance, you need to change the settings under "advanced options" as shown in the following example:

```
"advanced_options": {
  "ambari_config": {
    "core-site": {
      "fs.cos.s3service.endpoint": "MMMM",
      "fs.cos.s3service.access.key": "XXXX",
      "fs.cos.s3service.secret.key":"YYYY",
      "fs.cos.s3iamservice.iam.api.key":"ZZZZ",
      "fs.cos.s3iamservice.endpoint":"NNNN."
    }
  }
}
```

Include the advanced options settings under the "default_config" element when creating a serverless instance as shown in the following example:

```
"default_config": {
  "fs.cos.s3service.endpoint": "MMMM",
  "fs.cos.s3service.access.key": "XXXX",
  "fs.cos.s3service.secret.key":"YYYY",
  "fs.cos.s3iamservice.iam.api.key":"ZZZZ",
  "fs.cos.s3iamservice.endpoint":"NNNN."
}
```

Non-Spark parameters, like YARN are no longer applicable in serverless instance API calls.