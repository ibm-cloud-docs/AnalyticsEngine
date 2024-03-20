---

copyright:
  years: 2017, 2024
lastupdated: "2024-01-31"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Forwarding logs to instance home
{: #viewing-logs_1}

The platform logs are forwarded to instance home by default unless specified by the user. You can view the logs for a specific application from the `<instance_id>/logs/<app_id>` directory. You can also disable this feature.
{: shortdesc}

## Downloading the logs
{: #platform-logs_1}

To download the logs from the instance home, run the following command by using the AWS CLI. For more information about using CLI, see [Configure the CLI to connect to Object Storage](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-aws-cli#aws-cli-config).

```sh
aws --endpoint-url <bucket_endpoint> s3 cp s3://<bucket_name>/<instance_id>/logs/<app_id>/ ./ --recursive
```
{: codeblock}

Example :

```sh
aws --endpoint-url https://s3.us-south.cloud-object-storage.appdomain.cloud/ s3 cp s3://do-not-delete-ae-bucket-a0048029-c78a-439a-a864-14fdd3b2d95b/a0048029-c78a-439a-a864-14fdd3b2d95b/logs/2cd9036a-9e4a-4b79-81ec-2d8ae96d674c/ ./ --recursive
```
{: codeblock}


## Changing Spark Log Level
{: #enable-logging-from-iae_1}

You can edit the `spark log level configuration` to set different log levels like INFO, DEBUG for driver and executor logs that are forwarded to instance home.

Disable the logging feature completely by specifying the following driver and (or) executor log level.

```sh
ae.spark.driver.log.level = OFF
ae.spark.executor.log.level = OFF

```
{: codeblock}

For more information about configuring the log level information, see [Configuring Spark log level information](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-config_log).



By default the driver log level is `WARN` and the executor log level is `OFF`. You can view only the driver logs in your instance home, if the Spark log level configuration is not specified. To include the executor logs, change the log level to a different value than OFF. For more information, see [Configuring Spark log level information](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-config_log).
{: note}



Also, you can also choose to forward logs to {{site.data.keyword.la_full_notm}} service. For more information, see [Forwarding logs to {{site.data.keyword.la_full_notm}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-platform-logs). If the driver or executor log level information is configured as 'OFF', then logs are not forwarded to {{site.data.keyword.la_full_notm}} service even if it is enabled through the Logging APIs.
{: important}
