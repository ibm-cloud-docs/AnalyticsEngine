---

copyright:
  years: 2021, 2023
lastupdated: "2023-05-29"

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

## May 2023
{: #AnalyticsEngine-may23}

### 29 May 2023
{: #AnalyticsEngine-29may2023}

Removal of Python v3.9 support from Spark 3.1 and Spark 3.3 runtimes
: The IBM Analytics Engine - Serverless Spark application plans to discontinue Python v3.9 support from Spark 3.1 and Spark 3.3 runtimes by June15, 2023. Support for Python v3.10 is already deployed for Spark 3.1 and Spark 3.3 runtime.
Based on the workload, make sure that you test your spark application with the new version of Python, v 3.10 for any failures before June 15, 2023. Contact IBM Support for any issues. See the procedure for testing the Spark application [Run a Spark application with nondefault language version]( https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#run-a-spark-application-with-non-default-language-version).


<!-- The IBM Analytics Engine - Serverless Spark application now supports Python v 3.10. The previous version of Python, v3.9 will be revoked on June15, 2023. The IBM Analytics Engine - Serverless Spark application (Spark v3.1 and v3.2) does not support Python v3.9 there after and sets Python v3.10 as the default language for the IBM Analytics Engine - Serverless Spark application. See the [announcement](https://cloud.ibm.com/status/announcement?query=ANNOUNCEMENT%3A+Python+3.9+removal+from+IBM+Analytics+Engine+-+Serverless+Spark%0D%0A%0D) for reference.
Based on the workload, make sure that you test your spark application with the new version of Python, v 3.10 for any failures before June 15, 2023. Contact IBM Support for any issues. See the procedure for testing the Spark application [Run a Spark application with nondefault language version](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#use-env-varsadd). -->

### 25 May 2023
{: #AnalyticsEngine-25may2023}
{: release-note}

Pagination of application list in the REST API and CLI
: You can now limit the number of applications returned by the Analytics Engine serverless REST API endpoint, SDK method and CLI command for [listing applications](https://cloud.ibm.com/apidocs/ibm-analytics-engine/ibm-analytics-engine-v3#list-applications).
Use the query parameter limit to specify the number of applications to be returned and specify the value of `next.start` or `previous.start` from the API response as the value of the start query parameter to fetch the next or previous page of the results. The applications are listed in descending order based on the submission time, with the newest application being the first.

The pagination is an optional feature in this release. From the next release of the service, the results will be paginated by default.
{: note}

## 05 January 2023
{: #AnalyticsEngine-jan0523}
{: release-note}

Analyze application runs on the Spark history server
: You can now run a Spark history server on your {{site.data.keyword.iae_full_notm}} serverless instance.

    The Spark history server provides a Web UI to view Spark events that were forwarded to the {{site.data.keyword.cos_short}} bucket that was defined as the instance home. The Web UI helps you analyze how your Spark applications ran by displaying useful information like:

    - A list of the stages that the application goes through when it is run
    - The number of tasks in each stage
    - The configuration details such as the running executors and memory usage

    You are charged for the CPU cores and memory consumed by the Spark history server while it is running. The rate is $0.1475 USD per virtual processor core hour and $0.014 USD per gigabyte hour.

    See [Use the Spark history server](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-history-serverless).


## September 2022
{: #AnalyticsEngine-sep22}

### 21 September 2022
{: #AnalyticsEngine-sep2122}
{: release-note}

Support for Spark 3.3
:   You can now provision {{site.data.keyword.iae_full}} severless plan instances with the default Spark runtime set to Spark 3.3, which enables you to run Spark applications on Spark 3.3.


### 09 September 2022
{: #AnalyticsEngine-sep0922}
{: release-note}

You can now use Hive metastore to manage the metadata related to your applications tables, columns, and partition information when working with Spark SQL.
:   You could choose to externalize this metastore database to an external data store, like to an {{site.data.keyword.sqlquery_notm}} (previously SQL Query) or an {{site.data.keyword.databases-for-postgresql_full_notm}} instance. For details, see [Working with Spark SQL and an external metastore](/docs/AnalyticsEngine?topic=AnalyticsEngine-external-metastore).


## July 2022
{: #AnalyticsEngine-jul22}

### 12 July 2022
{: #AnalyticsEngine-jul1222}
{: release-note}

You can now provision {{site.data.keyword.iae_full}} serverless instances in a new region.
:   In addition to the {{site.data.keyword.Bluemix_short}} `us-south` (Dallas) region, you can now also provision serverless instances in the `eu-de` (Frankurt) region.


### 08 July 2022
{: #AnalyticsEngine-jul0822}
{: release-note}

New API for platform logging
: Start using the `log_forwarding_config` API to forward platform logs from an {{site.data.keyword.iae_full_notm}} instance to {{site.data.keyword.la_full_notm}}. Although you can still use the `logging` API, it is deprecated and will be removed in the near future. For details on how to use the `log_forwarding_config` API, see [Configuring and viewing logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-viewing-logs).


## 13 May 2022
{: #AnalyticsEngine-may1322}
{: release-note}

Support for Python 3.9
: You can now run Spark applications using Python 3.9. on your {{site.data.keyword.iae_full_notm}} serverless instances.


## 04 April 2022
{: #AnalyticsEngine-apr0422}
{: release-note}

Limitation on how long Spark applications can run
:   Spark applications can run for a maximum period of 3 days (72 hours). Any applications that run beyond this period will be auto-cleaned in order to adhere to the security and compliance patch management processes for applications in Analytics Engine.


## 30 March 2022
{: #AnalyticsEngine-mar3022}
{: release-note}

Start using the {{site.data.keyword.iae_short}} serverless CLI
:   Use this tutorial to help you get started quickly and simply with provisioning an {{site.data.keyword.iae_short}} serverless instance, and submitting and monitoring Spark applications. See [Create service instances and submit applications using the CLI](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-cli).


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
