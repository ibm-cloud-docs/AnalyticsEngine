---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-31"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Overview of IBM Analytics Engine serverless instances
{: #IAE-overview-serverless}

As the name indicates, the {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark gives you a serverless experience in which to run your Spark applications. You are billed only for the resources your Spark applications consume.

You can spin up a {{site.data.keyword.iae_full_notm}} serverless instance within seconds, customize it with library packages of your choice, and run Spark workloads. You get billed only for the duration of compute and the memory resources consumed by your Spark applications.

To keep your expenses under control, you can limit the resources that are used by your instance by setting a quota for CPU and memory consumption.

You can invite team members to work on a shared {{site.data.keyword.iae_full_notm}} serverless instance.

Currently, you can create {{site.data.keyword.iae_full_notm}}  serverless instances only in the US South region.

A prerequisite when creating {{site.data.keyword.iae_full_notm}} serverless Spark instances is that you must use an {{site.data.keyword.cos_full_notm}} bucket as the *instance storage volume* . This storage space is used to store Spark History events and custom library sets that need to be made available to your Spark applications. Currently, the Spark job logs, and the driver and executor logs are also stored in this {{site.data.keyword.cos_short}} bucket. At a later time, you will be able to  aggregate these logs to a centralized {{site.data.keyword.la_short}} server that you own.


- [Provisioning serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)
- [Customizing the instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-instance)
- [Running Spark batch applications](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-batch-serverless)
- [Managing user access to share Spark instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless)
