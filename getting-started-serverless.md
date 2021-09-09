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

# Getting started tutorial
{: #getting-started}

The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark offers the ability to spin up {{site.data.keyword.iae_full_notm}} serverless instances within seconds, customize them with library packages of your choice, and run your Spark workloads. <!-- You can invite team members to collaborate on these instances, all the while keeping resource expenses under control by setting quotas for CPU and memory consumption.-->

An instance is allocated compute and memory resources on demand  when Spark workloads are deployed. When an application is not in running state, no computing resources are allocated to the {{site.data.keyword.iae_full_notm}} instance. Pricing is based on the actual amount of resources consumed by the instance, billed on a per second basis.

Currently, you can create {{site.data.keyword.iae_full_notm}} serverless instances only in the US South region.

## Before you begin

To start running Spark applications in {{site.data.keyword.iae_full_notm}}, you need:

- An {{site.data.keyword.Bluemix}} account.
- Instance home storage space that is referenced from the {{site.data.keyword.iae_full_notm}}. This storage space is used to store Spark History events, which are created by your applications and any custom library sets, which need to be made available to your Spark applications. Currently, the Spark application events, and the driver and executor logs are also stored in this {{site.data.keyword.cos_short}} bucket. At a later time, you will be able to aggregate these logs to a centralized {{site.data.keyword.la_short}} server that you own.
- An serverless instance of the {{site.data.keyword.iae_full_notm}} service.

## Provision an instance and create a cluster

To provision an instance:

1. Get a basic understanding of the architecture and key concepts. See [Serverless instance architecture and concepts](/docs/analyticsengine?-serverless-architecture-concepts).
1. [Provision a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)

## Running applications

To run Spark applications in a serverless  {{site.data.keyword.iae_full_notm}} instance:

1. Optionally, give users access to the provisioned instance to enable collaboration. See [Managing user access to share instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
1. Optionally, customize the instance to fit the requirements of your applications. See [Customizing the instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-instance).
1. Submit your Spark application by using the Spark application REST API. See [Running Spark batch applications](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-batch-serverless).
1. Submit your Spark application by using the Livy batch API. See [Running Spark batch applications using the Livy API](/docs/analyticsengine?topic=AnalyticsEngine-livy-api-serverless).
