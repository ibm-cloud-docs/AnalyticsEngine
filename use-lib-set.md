---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-15"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:note: .note}
{:screen: .screen}
{:pre: .pre}

# Using a library set
{: #use-lib-set}

After you have created a library set, you can reference and consume it in your Spark applications. When you run your Spark application in the {{site.data.keyword.iae_full_notm}} instance, the library set is loaded from the instance home and is made available to the Spark application.

A library set is referenced in a Spark application using the `"ae.spark.librarysets"` parameter in the `"conf"` section of the Spark application submission payload.

To reference a library set when submitting a Spark application:

1. Get the IAM token. See [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Issue the following cURL command:
    ```sh
    curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer <IAM token>" -H "content-type: application/json"  -d @submit-spark-app.json
    ```
    {: codeblock}

    Example for submit-spark-app.json:
    ```json
    {
      "application_details": {
      "application": "cos://<bucket-name>.<cos-name>/my_spark_application.py",
      "arguments": ["arg1", "arg2"],
      "conf": {
        "spark.hadoop.fs.cos.<cos-name>.endpoint":"https://s3.us-south.cloud-object-storage.appdomain.cloud",
        "spark.hadoop.fs.cos.<cos-name>.access.key":"<access_key>",
        "spark.hadoop.fs.cos.<cos-name>.secret.key":"<secret_key>",
        "ae.spark.librarysets":"my_library_set"
        }
    }
    }
    ```
    {: codeblock}

    Currently, only one library set can be referenced during Spark application submission under the `"ae.spark.librarysets"` attribute.
    {: note}

    If the application is accepted, you will receive a response like the following:
    ```json
    {
      "id": "87e63712-a823-4aa1-9f6e-7291d4e5a113",
      "state": "accepted"
    }
    ```

1. Track the status of the application by invoking the application status REST API. See [Get the status of an application](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#spark-app-status).
