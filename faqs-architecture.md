---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-16"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}

# FAQs about the architecture
{: #faqs-architecture}

## Is {{site.data.keyword.Bluemix_notm}} Object Storage included in {{site.data.keyword.iae_full_notm}}?
{: faq}

No, {{site.data.keyword.Bluemix_notm}} Object Storage is not included. It is a separate offering. To learn more about {{site.data.keyword.Bluemix_notm}} Object Storage, see the [product documentation](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-about-ibm-cloud-object-storage) or the [documentation about its functionality](https://www.ibm.com/cloud/object-storage).

## How does {{site.data.keyword.Bluemix_notm}} Object Storage work in the {{site.data.keyword.iae_full_notm}} Hadoop environment?
{: faq}

Is it exactly equivalent to HDFS, only that it uses a different URL?

{{site.data.keyword.Bluemix_notm}} Object Storage implements most of the Hadoop File System interface. For simple read and write operations, applications that use the Hadoop File System API will continue to work when HDFS is substituted by {{site.data.keyword.Bluemix_notm}} Object Storage. Both are high performance storage options that are fully supported by Hadoop.

## What other components like {{site.data.keyword.Bluemix_notm}} Object Storage should I  consider while designing a solution using {{site.data.keyword.iae_full_notm}}?
{: faq}

In addition to {{site.data.keyword.Bluemix_notm}} Object Storage, consider using Compose  MySQL, available on {{site.data.keyword.Bluemix_notm}}, for persisting Hive metadata. When you delete a cluster, all data and metadata is lost. Persisting
Hive metadata in an external relational store like Compose allows  you to reuse this data again after clusters were deleted or access to clusters was denied.

{{site.data.keyword.iae_full_notm}} supports passing the location of metadata through customization scripts which you can use when starting a cluster. Hence, you can have the cluster pointing to the right metadata location as soon as it is spun up.

## How should I size my cluster?
{: faq}

Sizing a cluster is highly dependent on workloads. Here are some general guidelines:

For Spark workloads reading data from the object store, the minimum RAM in a cluster should be at least half the size of the data you want to analyze in any given job. For the best results, the recommended sizing for Spark workloads reading data from the object store is to have the RAM twice the size of the data you want to analyze. If you expect to have a lot of intermediate data, you should size the number of nodes to provide the right amount of HDFS space in the cluster.

## How do I design and size multiple environments for different purposes?
{: faq}

If you want to size multiple environments, for example a production environment with HA, a disaster recovery environment, a staging environment with HA, and a development environment, you need to consider the following aspects.

Each of these environments should use a separate cluster. If
you have multiple developers on your team, consider a separate
cluster for each developer unless they can share the same cluster credentials. For a development environment, generally, a cluster with  1 master and 2 compute nodes should suffice. For a staging environment where functionality is tested, a cluster with 1 master and 3 compute nodes is recommended. This gives you additional resources to test on a slightly bigger scale before deploying to production. For a disaster recovery environment with more than one cluster, you will need third party remote data replication capabilities.

Because data is persisted in Cloud Object Storage in {{site.data.keyword.iae_full_notm}}, you do need to have more than one cluster  running all the time. If the production cluster goes down, then a new cluster can be spun up using the DevOps tool chain and can be designated as the production cluster. You should use the customization scripts to configure the new cluster exactly like the previous production cluster.

## How is user management done in {{site.data.keyword.iae_full_notm}}?
{: faq}

How do I add more users to my cluster?

All clusters in {{site.data.keyword.iae_full_notm}} are single user, in other words, each cluster has only one Hadoop user ID with which all jobs
are executed. User authentication and access control is managed by the {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) service. After a user has logged on to {{site.data.keyword.Bluemix_notm}}, access to {{site.data.keyword.iae_full_notm}} is given or denied based on the IAM permissions set by the administrator.

A user can share his or her cluster’s user ID and password with other users; note however that in this case the other users have full access to the cluster. Sharing a cluster through a project in {{site.data.keyword.DSX_short}} is the recommended approach. In this scenario, an administrator sets up the cluster through the {{site.data.keyword.Bluemix_notm}} portal and *associates* it with a project in {{site.data.keyword.DSX_short}}. After this is done, users who have been granted access to that project can submit jobs through notebooks or other tools that requires a Spark or Hadoop runtime. An advantage of this approach is that user access to the {{site.data.keyword.iae_full_notm}} cluster or to any data to be analyzed can be controlled within {{site.data.keyword.DSX_short}}.

## How is data access control enforced in {{site.data.keyword.iae_full_notm}}?
{: faq}

Data access control can be enforced by using {{site.data.keyword.Bluemix_notm}} Object Storage ACLs (access control lists). ACLs in {{site.data.keyword.Bluemix_notm}} Object Storage are tied to the {{site.data.keyword.Bluemix_notm}} Identity and Access Management service.

An administrator can set permissions on a Cloud Object Storage bucket or on stored files. Once these permissions are set, the credentials of a user determine whether access to a data object through {{site.data.keyword.iae_full_notm}} can be granted or not.

In addition, all data in Cloud Object Storage can be cataloged using the IBM Watson Knowledge Catalog functionality. Governance policies can be defined and enforced using Data Catalog after the data was cataloged. Projects created in WDP can be used for a better management of user access control.

## Can I run a cluster or job for a long time?
{: faq}

Yes, you can run a cluster for as long as is required. However, to prevent data loss in case of an accidental cluster failure, you  should ensure that data is periodically written to {{site.data.keyword.Bluemix_notm}} Object Storage and that you don't use HDFS as a persistent store.

## More FAQs

- [General FAQs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-general-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-integration-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} operations](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-operations-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} security](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-security-faqs)
