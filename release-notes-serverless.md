---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-30"

keywords: IBM Analytics Engine release notes

subcollection: AnalyticsEngine

content-type: release-note

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}

<!-- keywords values above are place holders. Actual values should be pulled from the release notes entries. -->

{{site.data.keyword.attribute-definition-list}}

<!-- You must add the release-note content type in your attribute definitions AND to each release note H2. This will ensure that the release note entry is pulled into the notifications library. -->

# Release notes for {{site.data.keyword.iae_full_notm}} serverless instances
{: #iae-serverless-relnotes}

<!-- The title of your H1 should be Release notes for _service-name_, where _service-name_ is the non-trademarked short version conref. Include your service name as a search keyword at the top of your Markdown file. See the example keywords above. -->

Use these release notes to learn about the latest updates to {{site.data.keyword.iae_full_notm}} serverless instances that are grouped by date.
{: shortdesc}

<!-- If you also have a change log for your API or CLI, include the following tip with a link to the change log.
For information about changes to the _service-name_ API, see [Change log for _service-name_ API](/docs/link-to-change-log).
{: tip}  -->

## 30 March 2022
{: #AnalyticsEngine-mar3022}
{: release-note}

Start using the {{site.data.keyword.iae_short}} serverless CLI
: Use this tutorial to help you get started quickly and simply with provisioning an {{site.data.keyword.iae_short}} serverless instance, and submitting and monitoring Spark applications. See [Create service instances and submit applications using the CLI](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-cli).

<!--
## 24 March 2022
{: #AnalyticsEngine-mar2422}
{: release-note}

You can now run a Spark history server on your {{site.data.keyword.iae_full_notm}} serverless instance.
: [Use the Spark history server](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-history-serverless) to analyze the Spark applications that you have run with your serverless instance.
-->

## 9 September 2021
{: #AnalyticsEngine-sep0921}
{: release-note}

Introducing {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark
:   The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark offers the ability to spin up {{site.data.keyword.iae_full_notm}} serverless instances within seconds, customize them with library packages of your choice, and run your Spark workloads. 

New: The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark is now GA in the Dallas {{site.data.keyword.Bluemix_notm}} service region. 
:   This plan offers a new consumption model using Apache Spark whereby resources are allocated and consumed only when Spark workloads are running.

    Capabilities available in the {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark include:
    - Running Spark batch and streaming applications
    - Creating and working with Jupyter kernels for interactive use cases
    - Running Spark batch applications through an Apache Livy like interface
    - Customizing instance with your own libraries
    - Autoscaling Spark workloads
    - Aggregating logs of your Spark workloads to the {{site.data.keyword.la_short}}  server

    To get started using the serverless plan, see [Getting started using serverless {{site.data.keyword.iae_full_notm}} instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-getting-started).


