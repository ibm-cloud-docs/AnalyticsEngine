---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-18"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}


# General FAQs
{: #general-faqs}

## What is {{site.data.keyword.iae_full_notm}}?
{: #what-is-iae}
{: faq}

{{site.data.keyword.iae_full_notm}} provides a flexible framework to develop and deploy analytics applications on Hadoop and Spark. It allows you to spin up Hadoop and Spark clusters and manage them through their lifecycle.

## How is an {{site.data.keyword.iae_full_notm}} cluster different from a regular Hadoop cluster?
{: #difference-iae-cluster-and-hadoop}
{: faq}

{{site.data.keyword.iae_full_notm}} is based on an architecture which separates compute and storage. In a traditional Hadoop architecture, the cluster is used to both store data and perform application processing. In {{site.data.keyword.iae_full_notm}}, storage and compute are separated. The cluster is used for running applications and {{site.data.keyword.cos_full_notm}} for persisting the data. The benefits of such an architecture  include flexibility, simplified operations, better  reliability and cost effectiveness. Read this [whitepaper](https://www.ibm.com/downloads/cas/KDPB1REE){: external} to learn more.

## How do I get started with {{site.data.keyword.iae_full_notm}}?
{: #getting-started-with-iae}
{: faq}

{{site.data.keyword.iae_full_notm}} is available on {{site.data.keyword.Bluemix_notm}}. Follow this [link](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-getting-started) to learn more about the service and to start using it. You will also find tutorials and code samples to get you off to a fast start.

## Which distribution is used in {{site.data.keyword.iae_full_notm}}?
{: #distribution}
{: faq}

{{site.data.keyword.iae_full_notm}} is based on open source Hortonworks Data Platform (HDP). To find the currently supported version see the  [documentation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-IAE-overview#introduction).

## Which HDP components are supported in {{site.data.keyword.iae_full_notm}}?
{: #supported-hdp-components}
{: faq}

To see the full list of supported components and versions, see the [documentation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-IAE-overview#introduction).

## What node sizes are available in {{site.data.keyword.iae_full_notm}}?
{: #node-sizes}
{: faq}

To see the currently supported node sizes, see the [documentation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-IAE-overview#introduction).

## Why is there so little HDFS space on the clusters?
{: #hdfs-space}
{: faq}

What if I want to run a cluster that has a lot of data to be processed at one time?

The clusters in {{site.data.keyword.iae_full_notm}} are intended to be used as a compute clusters and not as persistent storage for data. Data should be persisted in [{{site.data.keyword.cos_full_notm}}](https://www.ibm.com/cloud/object-storage){: external}. This provides a more flexible, reliable, and cost effective way to build analytics applications. See this [whitepaper](https://www.ibm.com/downloads/cas/KDPB1REE){: external} to learn more about this topic. The Hadoop Distributed File System (HDFS) should be used at most only for intermediate storage during
processing. All final data (or even intermediate data) should be written to {{site.data.keyword.cos_full_notm}} before the cluster is deleted. If your intermediate storage requirements exceed the HDFS space  available on a node, you can add more nodes to the cluster.

## How many {{site.data.keyword.iae_full_notm}} clusters can I spin up?
{: #number-of-clusters}
{: faq}

There is no limit to the number of clusters you can spin up.

## Is there a free usage tier to try {{site.data.keyword.iae_full_notm}}?
{: #free-usage}
{: faq}

Yes, we provide the Lite plan which can be used free of charge.

When you move to a paid plan, you are entitled to $200 in credit that can be used against {{site.data.keyword.iae_full_notm}} or any service on {{site.data.keyword.Bluemix_notm}}. This credit is only allocated once.

## How does the Lite plan work?
{: #lite-plan}
{: faq}

The Lite plan provides 50 node-hours of free {{site.data.keyword.iae_full_notm}} usage. One cluster can be provisioned every 30 days. After the 50 node-hours are exhausted, you can upgrade to a paid plan within 24 hours to continue using the same cluster. If you do not upgrade within 24 hours, the cluster will be deleted and you have to provision a new one after the 30 day limit has passed.

A cluster created using a Lite plan has 1 master and 1 data node (2 nodes in total) and will run for 25 hours on the clock (50 hours/2 nodes). The node-hours cannot be paused, for example, you cannot use 10 node-hours, pause, and then come back and use the remaining 40 node-hours.

## What types of service maintenance exist in {{site.data.keyword.iae_full_notm}}?
{: #service-maintenance}
{: faq}

Occasionally, we need to update the {{site.data.keyword.iae_full_notm}} service. Most of these updates are non-disruptive and are performed when new features become available or when updates and fixes need to be applied.

Most updates that are  made to the system that handles service instance provisioning are non-disruptive. These updates include updates or enhancements to the service instance creation, deletion or management tools, updates or enhancements to the service management dashboard user interface, or updates to the service operation management tools.

Updates to the provisioned {{site.data.keyword.iae_full_notm}} clusters might include operating system patches and security patches for various components of the cluster. Again, many of these updates are non-disruptive.

However, if there is an absolute need to perform a disruptive deployment, you will be notified well in advance via email communication and on the [{{site.data.keyword.Bluemix_notm}} status page](https://cloud.ibm.com/status){: external}.

When a disruptive deployment is made to the system that handles the provisioning of a service instance, you will be unable to create, access, or delete an {{site.data.keyword.iae_full_notm}} service instance from the {{site.data.keyword.Bluemix_notm}} console or by using the service instance management REST APIs.
When a disruptive deployment is made to a provisioned service instance, you will not be able to access the {{site.data.keyword.iae_full_notm}} cluster or run jobs.

## More FAQs
{: #more-faqs-general}

- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-faqs-architecture)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-integration-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} operations](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-operations-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} security](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-security-faqs)
