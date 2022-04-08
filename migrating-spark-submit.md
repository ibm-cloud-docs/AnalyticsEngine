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

# Migrating from classic instances: Spark submit
{: #migrate-spark-submit}

This topic helps you when migrating from classic to serverless instances by providing a brief summary of the differences and sample code snippets for submitting Spark jobs.

## Using the Spark submit API

The following table summarizes the differences between classic and serverless instances when you use the Spark submit API.

| Classic instances | Serverless instances |
|-----------------------|--------------------------|
| - The `spark-submit` API call is CLI-based after SSH-ing to the cluster   \n - You can specify the deployment mode (external client or worker nodes on cluster) | - The `spark-submit` API call is available as REST API and CLI   \n - Supports token based authentication   \n - The deployment mode is set by default |

## Spark submit examples

When migrating to the serverless instances, you need to change the `spark-submit` API call. 

This is an example of how to submit a Spark application with arguments in a classic instance. The application file is stored in {{site.data.keyword.cos_full_notm}}.

```
spark-submit --conf "spark.hadoop.fs.cos.mycosservice.endpoint=https://MMM" --conf "spark.hadoop.fs.cos.mycosservice.access.key=XXX" --conf "spark.hadoop.fs.cos.mycosservice.secret.key=YYY" cos://mybucket.mycosservice/myapp.py arg1 arg2
```

In a serverless instance, you would need to submit the call as follows:
```
curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer $token" -H "content-type: application/json"  -d @submit-spark-app.json
``` 

Sample of the `submit-spark-app.json`:
```
{ 
    "application_details": { 
        "application": "cos://mybucket.mycosservice/myapp.py", 
        "arguments": ["arg1", "arg2"], 
        "conf": { 
            "spark.hadoop.fs.cos.mycosservice.endpoint": "https:/MMM, 
            "spark.hadoop.fs.cos.mycosservice.access.key": "XXX", 
            "spark.hadoop.fs.cos.mycosservice.secret.key": "YYY",
            "spark.app.name": "MySparkApp" }
    }
} 
```