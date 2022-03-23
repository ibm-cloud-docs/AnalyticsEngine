---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-19"

subcollection: analyticsengine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Security model
{: #security-model-serverless}

{{site.data.keyword.iae_full_notm}} serverless instances provide a security architecture that is designed to enable administrators and developers to create secure Spark clusters.

The following sections describe how the security model of {{site.data.keyword.iae_full_notm}} serverlesss instances manages the access to and control of the secure instances.

## Controlling access to {{site.data.keyword.iae_full_notm}} activities
{: #access-to-activities}

Access to {{site.data.keyword.iae_full_notm}} serverless instances is controlled by IAM authentication and authorization. IAM is the Identity and Access Management service of {{site.data.keyword.Bluemix_short}}. User authentication and access control happens through IAM when you log in with your IBMId. See how to [retrieve the IAM token](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token-serverless).

As an administrator or creator of the service instance, you can grant or deny access to other users with whom you may want to share the service instance. All activities on the service instance life cycle management, like modifying the instance configuration, submitting and tracking Spark applications or customizing the instance with custom library sets are controlled through IAM authentication and authorization. See [Granting permissions to users](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless) to understand which operations are supported and what is the level of access required for each of those operations.

## Encrypting at Rest
{: #encrypting-at-rest}

{{site.data.keyword.cos_full_notm}} is the recommended data store in which to store the data required for executing Spark applications on the instance. {{site.data.keyword.cos_full_notm}} offers encryption of the data stored in it. See Best practices for Cloud Object Storage encryption.
All scratch data in the cluster's local file system, either on the master or worker nodes, which might be used during Spark application execution is encrypted at disk level.

{{site.data.keyword.cos_full_notm}} is the recommended data store to store the data required for executing Spark jobs on the cluster.
{{site.data.keyword.cos_full_notm}} comes with default built-in encryption. See [Encrypting your data](/docs/cloud-object-storage/basics?topic=cloud-object-storage-encryption#encryption).

## Encrypting endpoints
{: #encrypting-endpoints}

All service endpoints to the cluster are SSL encrypted (TLS 1.2 enabled). In addition, when you use {{site.data.keyword.iae_full_notm}} with {{site.data.keyword.cos_full_notm}}, the link between the {{site.data.keyword.cos_short}} service instance and {{site.data.keyword.iae_full_notm}} is encrypted.

## Isolation and network access
{: #isolation-network-access}

Each {{site.data.keyword.iae_full_notm}} serverless instance gets is own isolated sandbox that is disconnected from other instances  from a network and security stand point.

Spark workloads deployed in an instance can:
- Communicate with other Spark workloads deployed in the same instance.
- Communicate with public internet
- Can connect with other {{site.data.keyword.Bluemix_short}} services over private end points

Spark workloads in one {{site.data.keyword.iae_full_notm}} instance cannot communicate with Spark workloads in another instance. See [Instance architecture](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#serverless-architecture) for more on instance isolation.

## Ensuring code security
{: #code-security}

You are advised to be cautious when applying libraries or package customization to your instance. You must use secure code from trusted sources only, so as not to compromise the overall security of the instances.

IBM recommends that you scan any source code, libraries, and packages you use before uploading them to your instance. While the use of non-trusted code will not impact others, it might impact you.