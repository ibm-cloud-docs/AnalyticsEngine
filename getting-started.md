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


In {{site.data.keyword.iae_full_notm}}, you can create two types of instances:

- **Serverless instances**: The Spark clusters that are created allocate machine resources on demand.  When an application is not in running state, no computing resources are allocated to the application. Pricing is based on the actual amount of resources consumed by an application.

  - [Serverless instance architecture and concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts)
  - [Provisioning a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)

- **Classic instances**: The classic Spark clusters that are created are allocated resources determined by a selected software package and hardware type (size). Pricing is set on a per hour per month basis, irrespective of whether applications are running and consuming resources.
  - [Classic instance architecture and concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-IAE-overview)
  - [Provisioning a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)

## Getting started using serverless {{site.data.keyword.iae_full_notm}} instances
{: #getting-started}

The {{site.data.keyword.iae_full_notm}} serverless plan offers the ability to spin up {{site.data.keyword.iae_full_notm}} serverless instances within seconds, customize them with library packages of your choice, and run your Spark workloads. You can invite team members to collaborate on these instances, all the while keeping resource expenses under control by setting quotas for CPU and memory consumption.

Currently, you can create {{site.data.keyword.iae_full_notm}} serverless instances only in the US South region.

## Before you begin

To start running Spark applications in {{site.data.keyword.iae_full_notm}}, you need:

- An {{site.data.keyword.Bluemix}} account.
- Instance volume storage space that is referenced from the {{site.data.keyword.iae_full_notm}} instance. For this instance storage, you need to create a bucket in an {{site.data.keyword.cos_full_notm}} instance. This storage space is used to store Spark History events, which are created by your applications and any custom library sets, which need to be made available to your Spark applications. Currently, the Spark job logs, and the driver and executor logs are also stored in this {{site.data.keyword.cos_short}} bucket. At a later time, you will be able to  aggregate these logs to a centralized {{site.data.keyword.la_short}} server that you own.
- An serverless instance of the {{site.data.keyword.iae_full_notm}} service.

## Running applications

To run Spark applications in a serverless  {{site.data.keyword.iae_full_notm}} instance:

1. Optionally, track the provisioning status of your instance. See [Tracking the status of the instance provisioning](/docs/AnalyticsEngine?topic=AnalyticsEngine-track-provisioning-serverless).
1. Optionally, give users access to the provisioned instance to enable collaboration. See [Managing user access to share instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
1. Optionally, customize the instance to fit the requirements of your applications. See [Customizing the instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-instance).
1. Submit your Spark application by using the Spark application REST API. See [Running Spark batch applications](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-batch-serverless).
1. Submit your Spark application by using the Livy batch API. See [Running Spark batch applications using the Livy API](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api-serverless).
