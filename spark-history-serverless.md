---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-05"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:note: .note}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Spark history server
{: #spark-history-serverless}

The Spark applications that are submitted on an {{site.data.keyword.iae_full_notm}} instance forward their Spark events to the {{site.data.keyword.cos_short}} bucket that was defined as the *instance home*. The Spark history server provides a Web UI to view these Spark events. The Web UI helps you to analyze how your Spark applications ran by displaying useful information like:

- A list of the stages that the application goes through when it is run
- The number of tasks in each stage
- The configuration details such as the running executors and memory usage

See the [Spark History server documentation](https://spark.apache.org/docs/latest/monitoring.html#viewing-after-the-fact){: external} for more details.

You can disable forwarding Spark events from a Spark application by setting the property `spark.eventLog.enabled` to `false` in the Spark application configuration.
{: note}

## Starting and stopping the Spark history server

Before accessing the Spark history server, you need to start the server. When you no longer need it, you should stop the server. You will be charged for the CPU cores and memory consumed by the Spark history server while it is running.

The Spark history server can be started and stopped by using:

- The [{{site.data.keyword.iae_short}} REST API](#rest-api)
- The [{{site.data.keyword.iae_short}} instance UI](#iae-ui)

<!-- - The [{{site.data.keyword.iae_short}} CLI](#iae-cli) -->


### Analytics Engine REST API
{: #rest-api}

You can use the {{site.data.keyword.iae_short}} REST API:

1. To view the status of the Spark history server

    ```sh
    curl "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_history_server" --header "Authorization: bearer <iam token>"
    ```
    {: codeblock}


1. To start the Spark history server

    ```sh
    curl --location --request POST "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_history_server" --header "Authorization: bearer <iam token>"
    ```
    {: codeblock}


1. To stop the Spark history server

    ```sh
    curl --location --request DELETE "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/spark_history_server" --header "Authorization: bearer <iam token>"
    ```
    {: codeblock}


<!--
### Analytics Engine CLI
{: #iae-cli}

You can use the {{site.data.keyword.iae_short}} CLI:

1. To view the status of the Spark history server

    ```sh
    ibmcloud ae-v3 history-server show
    ```
    {: codeblock}

1. To start the Spark history server

    ```sh
    ibmcloud ae-v3 history-server start
    ```
    {: codeblock}

1. To stop the Spark history server

    ```sh
    ibmcloud ae-v3 history-server stop
    ```
    {: codeblock}

-->

### {{site.data.keyword.iae_short}} instance UI
{: #iae-ui}

You can use the {{site.data.keyword.iae_short}} instance UI:
​
1. To view the Spark history server status:

    1. Open your resource list on [IBM Cloud](https://cloud.ibm.com/resources).
    2. Click **Services and software** and select your instance to open the details page.
    3. Select the **Spark history** tab. The current status of the server is shown on this page.

    If the status of the Spark history server is set to `Started`, you can also click **View Spark history** to launch the Web UI of the Spark history server in a new browser tab.

1. To start the Spark history server:

    1. On the `Spark history` page, click **Start history server**.
    2. Choose the server configuration and click **Start** to start the Spark history server.

1. To stop the Spark history server:

    1. On the `Spark history` page, click **Stop history server**.

## Opening the Spark history server Web UI

You can open the Spark history Web UI by opening the instance details page of your {{site.data.keyword.iae_short}} service instance, switching to the **Spark history** tab and clicking **View Spark history**.

Alternatively, the Spark history server Web UI URL can be obtained through a service endpoint that is made available to you as a service key (also known as a service credential). See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless).

Ensure that the Spark history server is running before you open the Web UI.

Log links under the Stages and Executors tabs of the Spark history server UI will not work as logs are not preserved with the Spark events. To review the task and executor logs, enable platform logging. For details, see [Configuring and viewing logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-viewing-logs).
{: note}

## Accessing the Spark history server REST API

In addition to the web UI, the Spark history server also provides a REST API which can be queried to view the Spark events generated by your Spark applications. The Spark history server REST API is available as a service endpoint in a service key (also known as service credential). See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless).

When you invoke the Spark history server REST API, you must specify your IAM token as a bearer token in the Authorization header.
E.g.
```sh
curl --location --request GET 'https://spark-console.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance id>/spark_history_api/v1/applications?status=completed' \
--header 'Authorization: Bearer <iam token>'
```
{: codeblock}

See the [Spark history server REST API documentation](https://spark.apache.org/docs/latest/monitoring.html#rest-api){: external} for more details.

## Customizing the Spark history server

By default, the Spark history server consumes 1 CPU core and 4 GiB of memory while it is running. If you want your Spark history server to use more resources, you can set the following properties in your Analytics Engine instance default configurations:

- `ae.spark.history-server.cores` for the number of CPU cores
- `ae.spark.history-server.memory` for the amount of memory

### Updating the CPU cores and memory settings using the REST API

You can update the CPU cores and memory settings using the {{site.data.keyword.iae_short}} REST API as follows:

```sh
curl --location --request PATCH "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_id>/default_configs" \
--header "Authorization: bearer <iam_token>" \
--header 'Content-Type: application/json' \
--data-raw '{
        "ae.spark.history-server.cores": "2",
        "ae.spark.history-server.memory": "8G"
}'
```
{: codeblock}


Only a pre-defined list of Spark driver and executor vCPU, and memory size combinations are supported. See [Supported Spark driver and executor vCPU and memory combinations](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits#cpu-mem-combination).


### Additional customisations

You can customize the Spark history server further by adding properties to the default Spark configuration of your {{site.data.keyword.iae_short}} instance. See standard [Spark history configuration options](https://spark.apache.org/docs/latest/monitoring.html#spark-history-server-configuration-options){: external}.

As this is a managed offering, you can't customize all of the standard Spark configuration options.
{: note}

For a list of the supported configurations and default values for settings, see [Default Spark configuration](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config).

## Best practices

Always stop the Spark history server when you no longer need to use it. Bear in mind that the Spark history server consumes CPU amd memory resources continuously while its state is `Started`.
