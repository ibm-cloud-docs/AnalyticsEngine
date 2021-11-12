---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-15"

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

Before you can submit a Spark application, you need to get authentication credentials and set the correct permissions on the Analytics Engine serverless instance.

1. You need the GUID of the service instance you noted down when you provisioned the instance. If you didn't make a note of the GUID, see [Retrieving the GUID of a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-instance-details).
1. You must have the correct permissions to perform the required operations. See [User permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
1. The Spark application REST APIs use IAM based authentication and authorization.

## Submitting a Spark application
{: #spark-submit-app}

{{site.data.keyword.iae_short}} Serverless provides you with a REST interface to submit Spark applications. The payload passed to the REST API maps to various command-line arguments supported by the `spark-submit` command. See [Parameters for submitting Spark applications](#spark-submit-parms) for more details.

When you submit a Spark application, you need to reference the application file. To help you to get started quickly and learn how to use the AE serverless Spark APIs, this section begins with an example that uses pre-bundled Spark application files that are referenced in the submit application API payload. The subsequent section shows you how to run applications that are stored in an {{site.data.keyword.cos_short}} bucket.

### Referencing pre-bundled files
{: #quick-start}

The provided sample application show you how to reference a `.py` word count application file and a data file in a job payload.

To learn how to quickly get started using pre-bundled sample application files:

1. Generate an IAM token if you haven’t already done so. See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Export the token into a variable:
    ```sh
    export token=<token generated>
    ```
    {: codeblock}

1. Prepare the payload JSON file. For example, submit-spark-quick-start-app.json:
    ```json
    {
      "application_details": {
        "application": "/opt/ibm/spark/examples/src/main/python/wordcount.py",
        "arguments": ["/opt/ibm/spark/examples/src/main/resources/people.txt"]
        }
    }
    ```
    {: codeblock}

1. Submit the Spark application:
    ```sh
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
    ```sh
    export token=<token generated>
    ```
   {: codeblock}

1. Prepare the payload JSON file. For example, `submit-spark-app.json`:
    ```json
    {
      "application_details": {
         "application": "cos://<application-bucket-name>.<cos-reference-name>/my_spark_application.py",
         "arguments": ["arg1", "arg2"],
         "conf": {
            "spark.hadoop.fs.cos.<cos-reference-name>.endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
            "spark.hadoop.fs.cos.<cos-reference-name>.access.key": "<access_key>",
            "spark.hadoop.fs.cos.<cos-reference-name>.secret.key": "<secret_key>",
            "spark.app.name": "MySparkApp"
         }
      
   }
   ```
   {: codeblock}

1. Submit the Spark application:
    ```curl
    curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer $token" -H "content-type: application/json"  -d @submit-spark-app.json
    ```
    {: codeblock}

    Sample response:
    ```json
    {
      "id": "87e63712-a823-4aa1-9f6e-7291d4e5a113",
      "state": "accepted"
    }
    ```

    Note:  
    - You can pass Spark application configuration values through the `"conf"` section in the payload. See [Parameters for submitting Spark applications](#spark-submit-parms) for more details.
    - `<cos-reference-name>` in the `"conf"` section of the sample payload is any name given to your {{site.data.keyword.cos_full_notm}} definition, which you are referencing in the URL in the `"application"` parameter.
    - It might take approximately a minute to submit the Spark application. Make sure to set sufficient timeout in the client code.
    - Make a note of the `"id"` returned in the response. You need this value to perform operations such as  getting the state of the application, retrieving the details of the application, or deleting the application.

## Passing the Spark configuration to an application
{: #pass-config}

You can use the `conf` section in the payload to pass the Spark application configuration. If you specified Spark configurations at the instance level, those are inherited by the Spark applications run on the instance, but can be overridden at the time a Spark application is submitted by including the `conf` section in the payload.

See [Spark configuration in {{site.data.keyword.iae_short}} Serverless](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config).

## Parameters for submitting Spark applications
{: #spark-submit-parms}

The following table lists the mapping between the `spark-submit` command parameters and their equivalent to be passed to the `application_details` section of the Spark application submission REST API payload.

| spark-submit command parameter | Payload to the Analytics Engine Spark submission REST API|
|----------------------------------|---------------------------------------|
| `<application binary passed as spark-submit command parameter>` | `application_details` -> `application` |
| `<application-arguments>` | `application_details` -> `arguments` |
| `class`| `application_details` -> `class` |
| `jars` | `application_details` -> `jars` |
| `name` |	`application_details` -> `name` or `application_details` -> `conf` -> `spark.app.name` |
| `packages`| `application_details` -> `packages`|
| `repositories`| `application_details` -> `repositories`|
| `files`| `application_details` -> `files`|
| `archives`| `application_details` -> `archives`|
| `driver-cores`| `application_details` -> `conf` -> `spark.driver.cores`|
| `driver-memory`| `application_details` -> `conf` -> `spark.driver.memory`|
| `driver-java-options`| `application_details` -> `conf` -> `spark.driver.defaultJavaOptions`|
| `driver-library-path` | `application_details` -> `conf` -> `spark.driver.extraLibraryPath`|
| `driver-class-path` | `application_details` -> `conf` -> `spark.driver.extraClassPath`|
| `executor-cores`| `application_details` -> `conf` -> `spark.executor.cores`|
| `executor-memory`| `application_details` -> `conf` -> `spark.executor.memory`|
| `num-executors`| `application_details` -> `conf` -> `ae.spark.executor.count`|
{: caption="Table 1. Mapping between the `spark-submit` command parameters and their equivalents passed to the `application_details` section of the Spark application REST API payload" caption-side="top"}


## Getting the state of a submitted application
{: #spark-app-status}

To get the state of a submitted application, enter:

```sh
curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/ spark_applications/<application_id>/state --header "Authorization: Bearer $token"
```
{: codeblock}

Sample response:
```json
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

```sh
curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/ spark_applications/<application_id> --header "Authorization: Bearer $token"
```
{: codeblock}

Sample response:
```json
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

To stop a submitted application, run the following:

```sh
curl -X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications/<application_id> --header "Authorization: Bearer $token"
```
{: codeblock}

Returns `204 – No Content`, if the deletion is successful. The state of the application is set to STOPPED.

This API is idempotent. If you attempt to stop an already completed or stopped application, it will still return 204.

You can use this API to stop an application in the following states: `accepted`, `waiting`, `submitted`, and `running`.
