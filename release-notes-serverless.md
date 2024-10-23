---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-21"

keywords: IBM Analytics Engine release notes

subcollection: AnalyticsEngine

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}


# Release notes for {{site.data.keyword.iae_full_notm}} serverless instances
{: #iae-serverless-relnotes}



Use these release notes to learn about the latest updates to {{site.data.keyword.iae_full_notm}} serverless instances that are grouped by date.
{: shortdesc}




## September 2024
{: #AnalyticsEngine-sept24}
{: release-note}

Deprecating the support for Spark 3.3 runtimes

: As of 28 March 2024, the IBM Log Analysis and IBM Cloud Activity Tracker services are deprecated and will no longer be supported as of 30 March 2025. You will need to migrate to IBM Cloud Logs, which replaces these two services, prior to 30 March 2025.


## August 2024
{: #AnalyticsEngine-aug24}
{: release-note}

Deprecating the support for Spark 3.3 runtimes

: Support for Spark 3.3 runtime in IBM Analytics Engine will be deprecated on Sep 17, 2024 and the default version will be changed to Spark 3.4 runtime. To ensure a seamless experience and to leverage the latest features and improvements, switch to Spark 3.4. To upgrade your instance to Spark 3.4, see [Replace Instance Default Runtime](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3#replace-instance-default-runtime).

## February 2024
{: #AnalyticsEngine-feb24}

### 02 February 2024
{: #AnalyticsEngine-02feb2024}
{: release-note}

Application logs are now available in the **instance home**
: The application logs are now forwarded to {{site.data.keyword.iae_short}} **instance home** by default. You can access the log information from the IBM Cloud Object Storage (COS) bucket. You can download the log file for any specific application from the path `<instance_id>/logs/<app_id>` for recording, sharing, and debugging purposes. For more information, see [Forwarding logs to instance home](/docs/AnalyticsEngine?topic=AnalyticsEngine-viewing-logs_1).

New location for logging Spark application events
: Starting 7 February 2024, the Spark application events are logged on a new path (`/<instance_id>/spark-events`) available in the instance-home bucket. To view the older applications on the Spark history interface, copy the Spark application events to a new location. For more information about copying the events, see [Spark history server](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-history-serverless#spark-history-serverless-6)

## December 2023
{: #AnalyticsEngine-dec23}

### 01 December 2023
{: #AnalyticsEngine-01dec2023}
{: release-note}

Encrypting internal network data for Spark workload
: You can now enable data encryption for the internal network data in transit (internal communication between components of the Spark application) by configuring the {{site.data.keyword.iae_full_notm}} properties at instance level or job level. For more information about encrypting internal network data for Spark workload, see [Encrypting internal network data for Spark workload](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-model-serverless#ency-spk-wrkld).


## November 2023
{: #AnalyticsEngine-nov23}

### 22 November 2023
{: #AnalyticsEngine-22nov2023}
{: release-note}


Configuring Spark log level information
: The default log level in the {{site.data.keyword.iae_full_notm}} Serverless Spark application, shall be changed to 'ERROR' by January 3, 2023. You can change the existing log configuration of logging at the 'INFO' level, to display a relevant and concise messages. For more information on changing the log level, see [Configuring Spark log level information](/docs/AnalyticsEngine?topic=AnalyticsEngine-config_log).


## October 2023
{: #AnalyticsEngine-oct23}

### 19 October 2023
{: #AnalyticsEngine-19oct2023}

Removal of development(*-devel) packages
: For security reasons, the *-devel packages (operating system development packages) are not pre-installed on the Spark runtime from now on. If you are already using the development packages,  the programs that use the development packages cannot be compiled . For any queries, contact IBM Support.


### 09 October 2023
{: #AnalyticsEngine-09oct2023}

Removal of Spark 3.1 support
: {{site.data.keyword.iae_full_notm}} no longer supports Spark 3.1. Upgrade your existing {{site.data.keyword.iae_full_notm}} instances to Spark 3.3 for the latest features and enhancements. For more information about the upgrade, see [Replace Instance Default Runtime](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3#replace-instance-default-runtime).

From the current release onwards, Spark 3.3 is the default runtime version for {{site.data.keyword.iae_full_notm}} instances.
{: note}

## September 2023
{: #AnalyticsEngine-sept23}

### 29 September 2023
{: #AnalyticsEngine-29sept2023}

Integration with {{site.data.keyword.lakehouse_short}}
: {{site.data.keyword.iae_full_notm}} now integrates with {{site.data.keyword.lakehouse_full}} to leverage the functional capabilities of {{site.data.keyword.lakehouse_short}}. For more information about the integration and to work with {{site.data.keyword.lakehouse_short}}, see [Working with {{site.data.keyword.lakehouse_short}}](AnalyticsEngine?topic=AnalyticsEngine-about-integration){: external}.


### 06 September 2023
{: #AnalyticsEngine-06sept2023}

Support for Spark 3.4
: You can now provision {{site.data.keyword.iae_full_notm}} severless plan instances with the default Spark runtime set to Spark 3.4, which enables you to run Spark applications on Spark 3.4.

## August 2023
{: #AnalyticsEngine-aug23}

### 23 August 2023
{: #AnalyticsEngine-23aug2023}

Deprecating the support of R v3.6 from Spark 3.1 and Spark 3.3 runtimes
: The IBM Analytics Engine deprecates the support for R v3.6 from Spark 3.1 and Spark 3.3 runtimes by September 6, 2023.
Support for R v4.2 is already deployed for Spark 3.1 and Spark 3.3 runtime. Make sure that you test your Spark application with the new version of R v4.2 for any failures before September 06, 2023. Contact IBM Support for any issues. To test the Spark application, see [Spark application REST API](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api).

### 09 August 2023
{: #AnalyticsEngine-09aug2023}

Deprecating the support for Spark 3.1
: The support for Spark 3.1 version on the {{site.data.keyword.iae_full_notm}} is deprecated and will be removed soon (by 09 October 2023). To ensure a seamless experience and to leverage the latest features and improvements, upgrade your existing {{site.data.keyword.iae_full_notm}} instances to Spark 3.3.
    To upgrade your instance to Spark 3.3, see [Replace Instance Default Runtime](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3#replace-instance-default-runtime).
    From this release onwards, Spark 3.3 will be the default runtime version for all new {{site.data.keyword.iae_full_notm}} instances created. This change enables you to benefit from the enhanced capabilities and optimizations available in the latest version.




## July 2023
{: #AnalyticsEngine-07july2023}
{: release-note}

### 07 July 2023
{: #AnalyticsEngine-07july2023}

Spark maintenance release version update for 3.3
: Spark applications with runtime set to Spark 3.3 will run internally using Spark 3.3.2 from now on. The patch version is now upgraded from 3.3.0 to 3.3.2.

## May 2023
{: #AnalyticsEngine-may23}

### 29 May 2023
{: #AnalyticsEngine-29may2023}

Removal of Python v3.9 support from Spark 3.1 and Spark 3.3 runtimes
: The IBM Analytics Engine - Serverless Spark application plans to discontinue Python v3.9 support from Spark 3.1 and Spark 3.3 runtimes by June15, 2023. Support for Python v3.10 is already deployed for Spark 3.1 and Spark 3.3 runtime.
Based on the workload, make sure that you test your spark application with the new version of Python, v 3.10 for any failures before June 15, 2023. Contact IBM Support for any issues. See the procedure for testing the Spark application [Run a Spark application with nondefault language version]( https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#run-a-spark-application-with-non-default-language-version).




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
