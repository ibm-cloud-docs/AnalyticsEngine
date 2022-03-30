---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-24"

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

- **Serverless instances**: An instance is allocated compute and memory resources on demand when Spark workloads are deployed. When an application is not in running state, no computing resources are allocated to the {{site.data.keyword.iae_full_notm}} instance. Pricing is based on the actual amount of resources consumed by the instance, billed on a per second basis.

  - [Serverless instance architecture and concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts)
  - [Provisioning a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)

- **Classic instances**: The classic {{site.data.keyword.iae_short}} clusters that are created are allocated resources determined by a selected software package and hardware type (size). Pricing is set on a per hour or per month basis, irrespective of whether applications are running and consuming resources.

  - [Classic instance architecture and concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-IAE-overview)
  - [Provisioning a classic instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE)

## Getting started using serverless {{site.data.keyword.iae_full_notm}} instances
{: #getting-started}

The {{site.data.keyword.iae_full_notm}} Standard Serverless plan for Apache Spark offers the ability to spin up {{site.data.keyword.iae_full_notm}} serverless instances within seconds, customize them with library packages of your choice, and run your Spark workloads.

Currently, you can create {{site.data.keyword.iae_full_notm}} serverless instances only in the US South region.

## Before you begin

To start running Spark applications in {{site.data.keyword.iae_full_notm}}, you need:

- An {{site.data.keyword.Bluemix}} account.
- Instance home storage in {{site.data.keyword.cos_full_notm}} that is referenced from the {{site.data.keyword.iae_full_notm}} instance. This storage is used to store Spark History events, which are created by your applications and any custom library sets, which need to be made available to your Spark applications. <!--Currently, the Spark application logs and the driver and executor logs are also stored in this {{site.data.keyword.cos_short}} bucket. At a later time, you will be able to aggregate these logs to a centralized {{site.data.keyword.la_short}} server that you own.-->
- An {{site.data.keyword.iae_full_notm}} serverless instance.

## Provision an instance and create a cluster

To provision an {{site.data.keyword.iae_full_notm}} instance:

1. Get a basic understanding of the architecture and key concepts. See [Serverless instance architecture and concepts](/docs/analyticsengine?-serverless-architecture-concepts).
1. [Provision a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)

## Run applications

To run Spark applications in a serverless {{site.data.keyword.iae_full_notm}} instance:

1. Optionally, give users access to the provisioned instance to enable collaboration. See [Managing user access to share instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).
1. Optionally, customize the instance to fit the requirements of your applications. See [Customizing the instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-instance).
1. Submit your Spark application by using the Spark application REST API. See [Running Spark batch applications](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-batch-serverless).
1. Submit your Spark application by using the Livy batch API. See [Running Spark batch applications using the Livy API](/docs/analyticsengine?topic=AnalyticsEngine-livy-api-serverless).

## End-to-end scenario using the {{site.data.keyword.iae_short}} serverless CLI

To help you get started quickly and simply with provisioning an {{site.data.keyword.iae_short}} instance and submitting Spark applications, you can use the {{site.data.keyword.iae_short}} serverless CLI. 

For an end-to-end scenario of the steps you need to take, from creating the services that are required, to submitting and managing your Spark applications by using the Analytics Engine CLI, see [Create service instances and submit applications using the CLI](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-cli).