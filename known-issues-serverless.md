---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-07"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}
{:help: data-hd-content-type='help'}

# Known issues and limitations
{: #known-issues-serverless}

This topic lists known issues and limitations that have been identified or were reported by users in error reports.

## Viewing the known issues
{: #view_known_issues_serverless}
{: help}
{: support}

The following table lists known issues:

| Category | Problem |  Workaround |
|---------|---------|------------|
| UI | If you have sensitive configurations at the instance level and you edit the default Spark configurations in {{site.data.keyword.iae_full_notm}} instance through the UI, existing sensitive data is saved as masked values (***) instead of the original values. This means that incorrect {{site.data.keyword.cos_short}} credentials are retrieved and 403 errors are returned while submitting application. | Use the REST API endpoint for updating default Spark configurations to update specific instance default configuration properties without affecting sensitive configurations. If you want to update configuration settings through the UI, re-enter the sensitive configuration values by replacing the masked values with real values. See [Update instance default Spark configurations](/apidocs/ibm-analytics-engine-v3#updateinstancedefaultconfigs). |
| UI | Special characters in the Spark configuration (specifically the `=` character) cannot be specified from the UI while creating or editing an instance. | Use the REST API endpoint for updating or replacing default Spark configurations if you have special characters like `=` in your configuration values. See [Update instance default Spark configurations](/apidocs/ibm-analytics-engine-v3#updateinstancedefaultconfigs). |
{: caption="Known issues and limitations in {{site.data.keyword.iae_short}} Serverless instances" caption-side="top"}
