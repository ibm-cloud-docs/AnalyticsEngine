---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-09"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Configuring and viewing logs
{: #viewing-logs}

Logging can help you troubleshoot issues in {{site.data.keyword.iae_full_notm}}.
{: shortdesc}

## Platform logs overview
{: #platform-logs}

`Platform logs` are logs that are exposed by {{site.data.keyword.iae_full_notm}} and the platform in {{site.data.keyword.cloud_notm}}. You must configure a logging instance in a region to monitor these logs.
{:shortdesc}

- Platform logs are regional.

    You can monitor logs from {{site.data.keyword.iae_full_notm}} on  {{site.data.keyword.cloud_notm}} in the region where the service is available.

- You can configure one instance only of the {{site.data.keyword.la_full_notm}} service per region to collect *platform logs* in that location.

    You can have multiple {{site.data.keyword.la_full_notm}} instances in a location. However, only 1 instance in a location (region) can be configured to receive logs from {{site.data.keyword.iae_full_notm}} in that {{site.data.keyword.cloud_notm}} location.
    {: important}

- To configure a logging instance, you must set on the *platform logs* configuration setting. Also, you must have the platform role `editor` or higher for the {{site.data.keyword.la_full_notm}} service in your account.

## Prerequisite to enable platform logging
{: #enable-logging-from-dashboard}

To view {{site.data.keyword.iae_full_notm}} platform logs, you must use the Observability dashboard in {{site.data.keyword.cloud_notm}} to configure platform logging. See [Configuring platform logs through the Observability dashboard](/docs/log-analysis?topic=log-analysis-config_svc_logs#config_svc_logs_ui) for the steps you need to follow to enable logging through the Observability dashboard.

After you have enabled platform logging through the Observability dashboard, proceed with the steps in the following section to enable forwarding  {{site.data.keyword.iae_full_notm}} platform logs.

The API calls in the following commands require the GUID of the service instance. If you didn't make a note of the GUID, see [Retrieving the GUID of a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-instance-details).

## Enabling platform logging from {{site.data.keyword.iae_full_notm}}
{: #enable-logging-from-iae}

To forward {{site.data.keyword.iae_full_notm}} platform logs to {{site.data.keyword.la_full_notm}}, invoke the following API against an existing {{site.data.keyword.iae_short}} instance:

```
curl -X PUT https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_guid>/logging -H "Authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"enable":true}'
```
{: codeblock}

## Disabling platform logging from {{site.data.keyword.iae_full_notm}}
{: #disable-loggingfrom-iae}

To disable forwarding {{site.data.keyword.iae_full_notm}} platform logs to {{site.data.keyword.la_full_notm}}, invoke the following API:

```
curl -X PATCH https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_guid>/logging -H "Authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"enable":false}'
```
{: codeblock}

## Retrieving the logging configuration of an instance
{: #log-config}

To see the current logging configuration of an {{site.data.keyword.iae_full_notm}} instance, invoke the following API:

```
curl -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_guid>/logging -H "Authorization: Bearer $TOKEN"
```
{: codeblock}

## Deleting the logging configuration of an instance
To delete logging configuration of an {{site.data.keyword.iae_full_notm}} instance, invoke the following API:

```
curl -X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance_guid>/logging -H "Authorization: Bearer $TOKEN"
```
{: codeblock}

## Viewing platform logs from the logging dashboard
{: #view-logs-dashboard}

When you run applications in {{site.data.keyword.iae_full_notm}} with logging enabled, logs are forwarded to an {{site.data.keyword.la_full_notm}} service where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.

To view the logs for an application, you must create an {{site.data.keyword.la_full_notm}} instance in the same region in which you created the {{site.data.keyword.iae_full_notm}} instance and {{site.data.keyword.la_short}}platform logs must be configured to receive {{site.data.keyword.iae_full_notm}} logging data.

To check for active {{site.data.keyword.la_short}}, see the [Observability dashboard](https://cloud.ibm.com/observe/logging).

## Analyzing {{site.data.keyword.iae_full_notm}} logs
{: #analyzing-logs}

From the {{site.data.keyword.la_short}} dashboard, you can filter logs forwarded from {{site.data.keyword.iae_full_notm}} using the following ways:

1. Click `Sources` on {{site.data.keyword.la_short}} dashboard and choose `ibmanalyticsengine` to get all logs from {{site.data.keyword.iae_full_notm}}.
1. To retrieve the logs for a specific Spark application, click **Sources** on the {{site.data.keyword.la_short}} dashboard, choose `ibmanalyticsengine`  and enter the search string `entity_type:application entity_id:<application_id>` in the search window at the bottom of the {{site.data.keyword.la_short}} dashboard.
1. Search for any keyword related to your Spark application in the search window.
