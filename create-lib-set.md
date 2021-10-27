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

# Creating a library set
{: #create-lib-set}

A library set is a collection of libraries that you can create and reference in Spark applications that consume the libraries. The library set is stored in the instance home storage associated with the instance at the time the instance is created.

Currently, you can only install Python packages through `conda` or `pip install`.

{{site.data.keyword.iae_short}} bundles a Spark application called `customize_instance_app.py` that you run to create a library set with your custom packages and can be consumed by your Spark applications.

**Prerequites**: To create a library set, you must have the permissions to submit a Spark application. See [User permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).

To create a library set:

1. Prepare a JSON file like the following:
    ```json
    {
      "library_set": {
        "action": "add",
        "name": "my_library_set",
        "libraries": {
          "conda": {
            "python": {
              "packages": ["numpy"]
              }
          }
      }
      }
    }
    ```
    {: codeblock}

    The description of the JSON attributes are as follows:
    - `"library_set"`: The top level JSON object that defines the library set.
    - `"action"`: Specifies the action to be taken for the library set. To create a library set, we use `"add"`. Currently, `"add"` is the only option supported.
    - `"name"`: Specifies the name with which the library set is identified. The created library set is stored in a file with this name in the {{site.data.keyword.cos_full_notm}} instance that you specified as `instance home`. **Important**: If you create more than one library set, you must use unique names for each set. Otherwise they will overwrite one another.
    - `"libraries"`: Defines a set of libraries. You can specify one or more libraries in this section. This element has one child JSON object per library package manager. Currently, only the `"conda"` and `"pip"` package managers are supported. Use `"pip"` or `"conda"` to install Python packages.
    - `"conda"`: Library package manager.
    - `"python"`: The library language. Currently, only Python is supported.
    - `"packages"`: List of packages to install.

1. Get the [IAM token](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).
1. Pass the JSON file as `"arguments"` in the following REST API call. Make sure that you escape the quotes as required, while passing to the REST API call.
    ```sh
    curl -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_applications --header "Authorization: Bearer <IAM token>" -H "content-type: application/json" -d @createLibraryset.json
    ```
    {: codeblock}

    Example for createLibraryset.json:
    ```json
    {
      "application_details": {
        "application": "/opt/ibm/customization-scripts/customize_instance_app.py",
        "arguments": ["{\"library_set\":{\"action\":\"add\",\"name\":\"my_library_set\",\"libraries\":{\"conda\":{\"python\":{\"packages\":[\"numpy\"]}}}}}"]
        }
    }
    ```
    {: codeblock}

    **Important**: You must escape all double quote characters in the strings that are passed as application arguments.

    If the application is accepted, you will receive a response like the following:
    ```
    {
      "application_id": "87e63712-a823-4aa1-9f6e-7291d4e5a113",
      "state": "RUNNING",
      "start_time": "Thursday 19 November 2020 17:37:02.380+0000"
    }
    ```
    When the state turns to FINISHED, the library set creation is complete.
1. Track the status of the application by invoking the application status REST API. See [Get the status of an application](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#spark-app-status).
