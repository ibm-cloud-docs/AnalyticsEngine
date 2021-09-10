---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-08"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}

# Spark application REST API
{: #spark-app-rest-api}

The {{site.data.keyword.iae_full_notm}} serverless plan provides REST APIs to submit and manage Spark applications. The following operations are supported:

1. [Get the required credentials and set permissions](#spark-app-creds).
1. [Submit the Spark application](#spark-submit-app).
1. [Retrieve the state of a submitted Spark application](#spark-app-status).
1. [Retrieve the details of a submitted Spark application](#spark-app-details).
1. [Stop a running Spark application](#spark-app-stop).


For a description of the available APIs, see the [{{site.data.keyword.iae_full_notm}} REST APIs for the serverless plan](https://test.cloud.ibm.com/apidocs/ibm-analytics-engine-v3#introduction).

The following sections in this topic show samples for each of the Spark application management APIs.

## Required credentials and permissions
{: #spark-app-creds}

Before you can submit a Spark application, you need to get authentication credentials and set the correct permissions on the  Analytics Engine serverless instance.

1. You need the GUID of the service instance you noted down when you provisioned the instance. If you didn't make a note of the GUID, see [Retrieving the GUID of a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-instance-details).
1. You must have the correct permissions to perform the required operations. See [User permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
1. The Spark application REST APIs use IAM based authentication and authorization.

## Submitting a Spark application
{: #spark-submit-app}

When you submit a Spark application, you need to reference the application file. To help you to get started quickly and learn how to use AE serverless Spark APIs, this section begins with an example that uses pre-bundled Spark application files that are referenced in the submit application API payload. The subsequent section shows you how to run applications that are stored in an {{site.data.keyword.cos_short}} bucket.


<!--
 You can save your Spark application file to:

- [A GitHub repository](#file-github)
- [An AWS S3 bucket](#file-aws)
- [An {{site.data.keyword.cos_full_notm}} bucket](#file-cos)

### Referencing files from GitHub
{: #file-github}

### Referencing files from an AWS S3 bucket
{: #file-aws}
-->

### Referencing pre-bundled files
{: #quick-start}

The provided sample application show you how to reference a .py word count application file and a data file in a job payload.

To learn how to quickly get started using pre-bundled sample application files:

1. Generate an IAM token if you haven’t already done so. See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Export the token into a variable:
   ```
   export token=<token generated>
   ```
   {: codeblock}

1. Prepare the payload JSON file. For example, submit-spark-quick-start-app.json:
   ```
   {
     "application_details": {
       "application": "/opt/ibm/spark/examples/src/main/python/wordcount.py",
       "arguments": ["/opt/ibm/spark/examples/src/main/resources/people.txt"]
       }
   }
   ```
   {: codeblock}

1. Submit the Spark application:
   ```
   curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer $token" -H "content-type: application/json"  -d @submit-spark-quick-start-app.json
   ```
   {: codeblock}


### Referencing files from an {{site.data.keyword.cos_short}} bucket
{: #file-cos}

To submit a Spark application:

1. Create a bucket for your application file. See [Bucket operations](/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) for details on creating buckets.
1. Add the application file to the newly created bucket. See [Upload an object](/docs/cloud-object-storage?topic=cloud-object-storage-object-operations#object-operations-put) for adding your application file to the bucket.
1. Generate an IAM token if you haven’t already done so. See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Export the token into a variable:
   ```
   export token=<token generated>
   ```
   {: codeblock}

1. Prepare the payload JSON file. For example, submit-spark-app.json:
   ```
   {
     "application_details": {
       "application": "cos://<application-bucket-name>.<cos-reference-name>/my_spark_application.py",
       "arguments": ["arg1", "arg2"],
       "conf": {
         "spark.hadoop.fs.cos.<cos-reference-name>.endpoint": "https://s3.private.us-south.cloud-object-storage.appdomain.cloud",
         "spark.hadoop.fs.cos.<cos-reference-name>.access.key": "<access_key>",
         "spark.hadoop.fs.cos.<cos-reference-name>.secret.key": "<secret_key>",
         "spark.app.name": "MySparkApp"
         },  
       "env": {
         "SPARK_ENV_LOADED": "2",
         "LD_LIBRARY_PATH": "/some/path/to/libs"
        }
     }
   }
   ```
   {: codeblock}

1. Submit the Spark application:
   ```
   curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer $token" -H "content-type: application/json"  -d @submit-spark-app.json
   ```
   {: codeblock}

   Sample response:
   ```
   {
     "id": "87e63712-a823-4aa1-9f6e-7291d4e5a113",
     "state": "accepted"
   }
   ```

   Note:  
   - You can pass Spark application configuration values through the `"conf"` section of the payload.
   - `<cos-reference-name>` in the `"conf"` section of the sample payload is any name given to your {{site.data.keyword.cos_full_notm}} definition, which you are referencing in the URL in the `"application"` parameter.
   - It might take approximately a minute to submit the Spark application. Make sure to set sufficient timeout in the client code.
   - Make a note of the `"id"` returned in the response. You need this value to perform operations such as  getting the state of the application, retrieving the details of the application, or deleting the application.


## Getting the state of a submitted application
{: #spark-app-status}

To get the state of a submitted application, enter:
   ```bash
   curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications/<application_id>/state --header "Authorization: Bearer $token"
   ```
   {: codeblock}

   Sample response:
   ```
   {
     "id": "a9a6f328-56d8-4923-8042-97652fff2af3",
     "state": "finished",
     "start_time": "2020-11-25T14:14:31.311+0000",
     "finish_time": "2020-11-25T14:30:43.625+0000"
   }
   ```

## Getting the details of a submitted application
{: #spark-app-details}

To get the details of a submitted application, enter:
   ```bash
   curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications/<application_id> --header "Authorization: Bearer $token"
   ```
   {: codeblock}

   Sample response:
   ```
   {
     "application_details": {
       "application": "/opt/ibm/spark/examples/src/main/python/wordcount.py",
       "arguments": [
       "/opt/ibm/spark/examples/src/main/resources/people.txt"
       ]},
       "id": "a9a6f328-56d8-4923-8042-97652fff2af3",
       "state": "finished",
       "start_time": "2020-11-25T14:14:31.311+0000",
       "finish_time": "2020-11-25T14:15:53.205+0000"
   }
   ```

## Stopping a submitted application
{: #spark-app-stop}

To stop a submitted application, enter:
   ```
   curl -X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications/<application_id> --header "Authorization: Bearer $token"
   ```
   {: codeblock}

   Returns `204 – No Content`, if the deletion is successful. The state of the application is set to STOPPED.

   This API is idempotent. If you attempt to stop an already completed or stopped application, it will still return 204.
