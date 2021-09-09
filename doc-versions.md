---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# IBM Analytics Engine plan types
{: #IAE-overview}

When you provision an {{site.data.keyword.iae_full_notm}} service instance, you can select between two plan types:

- Severless plans

  The Spark cluster created for a serverless plan automatically starts up, shuts down again after the job has run, and is configured uniquely for the needs of your job. Serverless Spark clusters are purpose-built for particular analytics use cases.
- Classic plans

  The cluster created for a classic plan starts up automatically but is shut down only when explicitly deleted. The cluster is configured for many analytics use cases which means that many preconfigured libraries and packages are never used by many jobs.

The {{site.data.keyword.iae_full_notm}} documentation differs substantially depending on which plan type you have selected. When browsing the documentation, make sure that you are in the designated section for your plan type.
