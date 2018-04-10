---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Getting started tutorial

{{site.data.keyword.iae_full_notm}} provides a flexible framework to develop and deploy analytics applications in Apache Hadoop and Apache Spark. It allows you to create and manage clusters using the {{site.data.keyword.Bluemix_short}} interface or using the Cloud Foundry CLI and REST APIs.

## Before you begin

* Get a quick overview of {{site.data.keyword.iae_full_notm}} in this short [video](https://developer.ibm.com/clouddataservices/docs/analytics-engine/).

## Provision an instance and create a cluster
* Watch how to [get started using {{site.data.keyword.iae_full_notm}}](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started).

  In this video you will learn how to provision an {{site.data.keyword.iae_full_notm}} cluster from IBM Cloud, find out about options to manage the cluster, and see how to connect {{site.data.keyword.DSX_short}} to {{site.data.keyword.iae_full_notm}} to analyze data.

 For details about the supported plans and on how to provision and configure your cluster, see the [{{site.data.keyword.iae_full_notm}} documentation](./provisioning.html#provisioning-an-analytics-engine-service-instance).

* Watch how to [provision an {{site.data.keyword.iae_full_notm}} service instance through {{site.data.keyword.DSX_short}}](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started/#provision). 

## Run applications on the cluster

* Watch a [demo](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started/#spark-notebook) and run through the tutorial using sample code and data. Download this [notebook](https://github.com/sharynr/notebooks/blob/master/Exploring%20Heating%20Problems%20in%20Manhattan.ipynb) and [sample data file](https://github.com/wdp-beta/get-started/blob/master/data/IAE_examples_data_311NYC.zip) to try it for yourself!
* Learn how to use [spark-submit](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started/#spark-submit) to execute a Python script on an {{site.data.keyword.iae_full_notm}} cluster.
* Learn how to programmatically use {{site.data.keyword.iae_full_notm}} through this [tutorial](https://github.com/IBM-Cloud/IBM-Analytics-Engine). Get access to sample scripts to start operationalizing your first applications.
* Get answers to some [frequently asked questions](./faq.html#frequently-asked-questions) about using {{site.data.keyword.iae_full_notm}}.

## Next steps
Now that you have provisioned a service instance and have created a cluster, you can start running jobs and managing your cluster:

- [Run Hadoop MapReduce jobs](/docs/services/AnalyticsEngine/hadoop-mapreduce-jobs.html).
- [Run Spark Interactive jobs](/docs/services/AnalyticsEngine/spark-interactive-notebooks-api.html).
- [Run Spark Batch jobs](/docs/services/AnalyticsEngine/Livy-api.html).
- Manage your cluster by using the [Cloud Foundry command line interface (cf CLI)](/docs/services/AnalyticsEngine/WCE-CLI.html) for various operations.
