---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-13"

subcollection: AnalyticsEngine

---


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
| Log forwarding | Logs from the {{site.data.keyword.iae_full_notm}} service are not being forwarded to an {{site.data.keyword.la_full_notm}} instance although it was configured for receiving platform logs. This can happen if your account is a child account of an enterprise account. See [What is an enterprise?](/docs/account?topic=account-what-is-enterprise). The logs might be getting forwarded to the oldest {{site.data.keyword.la_full_notm}} instance that was configured as a platform logs receiver in either the parent or another child account. This is a limitation in the {{site.data.keyword.la_full_notm}} supertenancy model. | Examine your account hierarchy and request access to the {{site.data.keyword.la_full_notm}} instance, or work with a non-enterprise account. |
|Application submission| The Spark application fails when the default configuration property spark.app.name at the instance has Chinese characters. This occurs because the configuration property, spark.app.name is defined at the instance level rather than being specified with the application payload during submission. The property at instance default configuration will only allow tagging individual applications and is not considered for application submission, resulting in the application failing to submit.| Setting the property spark.app.name in the payload of application will resolve the issue.|
{: caption="Known issues and limitations in {{site.data.keyword.iae_short}} Serverless instances" caption-side="top"}
