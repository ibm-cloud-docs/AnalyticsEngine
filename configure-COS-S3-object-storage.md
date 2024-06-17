---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-03"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with {{site.data.keyword.cos_short}}
{: #config-cluster-cos}

{{site.data.keyword.cos_full_notm}} is a highly scalable cloud storage service, designed for high durability, resiliency and security. See [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage).

This topic explains how to configure an {{site.data.keyword.iae_full_notm}} cluster to connect to data and applications stored in {{site.data.keyword.cos_short}}.

You should use {{site.data.keyword.cos_full_notm}} as your primary data source and sink as described in [Best Practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices). Not only the data itself, but also your application or job binaries, for example Spark Python files or Yarn application JARs, should reside in {{site.data.keyword.cos_short}}. By removing all data from the cluster, you make your cluster stateless, which gives you the flexibility to spin up new {{site.data.keyword.iae_full_notm}} clusters as you need them. See [Choose the right {{site.data.keyword.cos_short}} Storage configuration](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#encryption).

To work with {{site.data.keyword.cos_short}} in an {{site.data.keyword.iae_full_notm}} cluster:

1. Provision an {{site.data.keyword.cos_short}} service instance in {{site.data.keyword.Bluemix_short}}. See [Creating a new service instance](/docs/cloud-object-storage/iam?topic=cloud-object-storage-provision).

    Create an {{site.data.keyword.cos_short}} bucket in the  region of your choice and select the configuration, such as  resiliency (regional or cross-regional), and storage volume and price. See [Creating buckets to store your data](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage).

    Make a note of the bucket name that you created. You will need it later when you configure {{site.data.keyword.iae_full_notm}} to work with {{site.data.keyword.cos_short}}.
1. [Get the {{site.data.keyword.cos_short}} credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-get-cos-credentials).
1. [Determine the {{site.data.keyword.cos_short}} credentials for {{site.data.keyword.iae_full_notm}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-cos-credentials-in-iae).
1. [Configure {{site.data.keyword.iae_full_notm}} to use {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-configure-iae-with-cos).
1. [Access objects in {{site.data.keyword.cos_short}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-access-objs-in-cos).



