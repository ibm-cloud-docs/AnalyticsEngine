---

copyright:
  years: 2017
lastupdated: "2017-08-04"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Getting started

IBM Analytics Engine on Bluemix enables you to provision  clusters for Hadoop and Spark workloads. You can  customize the cluster by modifying the Hadoop configuration, installing third party Python and R packages, installing yum packages from CentOS repositories, or running custom scripts on specified nodes of the cluster.

To create and start using an Analytics Engine cluster:

1. [Create an Analytics Engine cluster](./provisioning.html). Optionally, [customize the cluster](./customizing-cluster.html) with required libraries or any custom action.
2. [Fetch credentials and service end points](./Retrieve-service-credentials-and-service-end-points.html).
3. [Run Hadoop MapReduce jobs](./hadoop-mapreduce-jobs.html).
4. [Run Spark Interactive Jobs](./spark-interactive-notebooks-api.html).
5. [Run Spark Batch Jobs](./Livy-api.html).
6. Manage clusters using the [Cloud Foundry Command Line Interface (cf CLI)](./WCE-CLI.html) for various operations.
7. [Delete the service instance](./delete-instance.html) when jobs are finished.
