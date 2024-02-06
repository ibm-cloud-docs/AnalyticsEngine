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

# Overview
{: #viewing-logs}

When you submit applications or Spark workloads in {{site.data.keyword.iae_full_notm}}, you can monitor the application execution. {{site.data.keyword.iae_full_notm}} allows to debug the application errors or trace the application execution by monitoring the application generated logs as well as the Spark generated logs. Log analysis together with monitoring feature of **Spark History** page provides better troubleshooting of the applications.

{{site.data.keyword.iae_full_notm}} allows you to forward applications logs to the **instance home** or (and) to {{site.data.keyword.la_full_notm}} service.

## Default Behaviour
{: #viewing-logs_1}


When you run applications in {{site.data.keyword.iae_full_notm}}, the application logs (only `Spark Driver ERROR logs`) are forwarded to IBM Cloud Object Storage by default. You can access the log information from your **instance home**. For more information, see [instance home](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-cos-concepts).


## Forwarding logs to instance home
{: #viewing-logs-2}


Application Logs are forwarded to the {{site.data.keyword.iae_short}} **instance home** by default. You can access the log information from the IBM Cloud Object Storage (COS) bucket. You can download the log file for any specific application from the COS bucket for recording, sharing, and debugging purpose. If you want to change the default behaviour to include executor logs or INFO logs for driver or executor, you need to change the configurations. For more information, see [Forwarding logs to instance home]((/docs/AnalyticsEngine?topic=AnalyticsEngine-log_frwd_inst)).

## Forwarding logs to {{site.data.keyword.la_full_notm}}
{: #viewing-logs-3}


{{site.data.keyword.la_full_notm}} service allows you to view indexed logs, enable full-text search through all generated messages and query based on specific fields. Enabling the log forwarding feature forward the logs to {{site.data.keyword.la_full_notm}} service (in addition to **instance home**). Only the `spark driver ERROR logs` are forwarded by default. To change the behaviour to include executor logs, enable it by modifying the payload in the log forwarding API.For more information, see [Forwarding logs to {{site.data.keyword.la_full_notm}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-platform-logs)

## Changing the Spark Log Levels
{: #viewing-logs-4}


To change the default behaviour to forward logs at different log levels (INFO, DEBUG etc), see [Configuring Spark log level information](/docs/AnalyticsEngine?topic=AnalyticsEngine-config_log).


## Disabling Logging feature
{: #viewing-logs-5}


To disable sending log information to both **instance home** and {{site.data.keyword.la_full_notm}}, set the driver and executor log level configuration to OFF. For more information see, [Configuring Spark log level information](/docs/AnalyticsEngine?topic=AnalyticsEngine-config_log).
