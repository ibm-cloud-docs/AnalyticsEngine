---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-24"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Security model
{: #security-model}

{{site.data.keyword.iae_full_notm}} provides a security architecture that is designed to enable administrators and developers to create secure clusters on designated servers.

The following sections describe how the {{site.data.keyword.iae_full_notm}} security model manages the access to and control of the clusters.

## Controlling access to {{site.data.keyword.iae_full_notm}} activities

Typically, you will need to authenticate to access the {{site.data.keyword.iae_full_notm}} service instance for two different kinds of activities:

- Administrative activities such as adding nodes, tracking provisioning status, tracking the cluster customization request status, deleting a cluster, resetting the cluster password and even provisioning a server instance. These are classified as service instance activities.

    **Administrative activities** on the cluster require **IAM  authentication**. IAM is the Identity and Access Management service of {{site.data.keyword.Bluemix_short}}. User authentication and access control happens through IAM when you log in with your IBMId. See how to [retrieve the IAM token](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token). As an admin or creator of the service instance, you can grant or deny access to  other users with whom you may want to share the service instance.
- Activities around Spark and Hadoop services such as submitting a Spark job, connecting to a Hive JDBC endpoint, accessing job logs, and so on. These are classified as server or cluster activities.

    **Server or cluster activities** require **user-based authentication**. See [retrieving the cluster credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-cluster-credentials) to get the credentials.

## Encrypting at Rest

IBM Cloud Object Storage is the recommended data store to store the data required for executing Spark jobs on the cluster. IBM Cloud Object Storage offers encryption of the data stored in it.
See [Best practices for Cloud Object Storage encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#encryption).

All scratch data in the cluster's local file system, either on the management or data nodes, which might be used during job execution, or any other data that you might have in a directory or in HDFS is encrypted at disk level.

## Encrypting endpoints

All service endpoints to the cluster are SSL encrypted (TLS enabled). Access control and authentication to each of the REST endpoints is enabled through the Apache Knox secure gateway. In addition, when you use {{site.data.keyword.iae_full_notm}} with IBM Cloud Object Storage, the link between the Object Storage service instance and {{site.data.keyword.iae_full_notm}} is encrypted.

## Encrypting data in transit

You can enable encryption for data in transit for Spark jobs by explicitly configuring the cluster using a combination of Advanced Options and customization. See [Enabling Spark jobs encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-encryption) for how to configure the cluster to encrypt data-in-transit or inter node communication when executing Spark jobs.

## Using private endpoints to the cluster

When you provision a cluster, you can choose to have it enabled for private endpoints by using the {{site.data.keyword.Bluemix_short}} service endpoints integration feature. This feature allows you to securely access your {{site.data.keyword.iae_full_notm}} instances over the {{site.data.keyword.Bluemix_short}} private network. See [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).

## Permitting only single user access to clusters

Each {{site.data.keyword.iae_full_notm}} cluster is single user which means that each cluster has only one user ID through which all jobs must be executed. If you want to share cluster access with other users, you can only do this by sharing the cluster’s user ID and password. As an admin or creator of the {{site.data.keyword.iae_full_notm}} service instance, you can set [access permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions) for other users with whom you want to share the service instance. This way, for example, one or more users can execute their notebooks on the same shared {{site.data.keyword.iae_full_notm}} service instance. In IBM Watson Studio, you can share an {{site.data.keyword.iae_full_notm}} cluster with other users by adding those users as collaborators to your Watson Studio project.

Additionally, the IBM Cloud Object Storage bucket in which you save data and your Spark jobs can also be shared. So for example, if user A needs to access the output produced by user B, then user B should store that output in Cloud Object Storage and give user A access. See [Granting access permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions) for details.

## Isolating clusters and applying data sanitization methods during  cluster deletion

{{site.data.keyword.iae_full_notm}} clusters run on machines that are single tenant and hence each user has a dedicated cluster.

IBM uses the multi-pass DoD grade algorithm (5220.22M standard) for data destruction on the server when a cluster is deleted.

## Ensuring the security of your cluster with your code

You are advised to be cautious when applying libraries or package customization to your cluster. You must use secure code from trusted sources only so as not to compromise the overall security of the cluster.

IBM recommends that you scan any source, libraries, and packages you use before uploading them to your cluster.

While the use of non-trusted code will not impact other customers, it might impact you.
