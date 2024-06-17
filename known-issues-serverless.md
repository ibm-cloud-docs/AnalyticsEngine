---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-07"

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
{: caption="Known issues and limitations in {{site.data.keyword.iae_short}} Serverless instances" caption-side="top"}
