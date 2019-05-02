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


# Integration FAQs
{: #integration-faqs}

## Which other {{site.data.keyword.Bluemix_notm}} services can I use with {{site.data.keyword.iae_full_notm}}?
{: #iae-with-other-services}
{: faq}

{{site.data.keyword.iae_full_notm}} is a compute engine offered in {{site.data.keyword.DSX_full}} and can be used to push {{site.data.keyword.DSX_short}} jobs to {{site.data.keyword.iae_full_notm}}. Data can be written to Cloudant or Db2 Warehouse on Cloud after being processed by using Spark.

## How is {{site.data.keyword.iae_full_notm}} integrated with IBM Watson Studio?
{: #iae-ws}
{: faq}

{{site.data.keyword.iae_full_notm}} is a first class citizen in {{site.data.keyword.DSX_full}}. Projects (or individual notebooks) in
{{site.data.keyword.DSX_short}} can be associated with {{site.data.keyword.iae_full_notm}}. Once you have an
IBM Analytics cluster running in {{site.data.keyword.Bluemix_notm}}, log in to {{site.data.keyword.DSX_short}} using the same {{site.data.keyword.Bluemix_notm}} credentials you used for {{site.data.keyword.iae_full_notm}}, create a project, go to the project's Settings page, and then add  the {{site.data.keyword.iae_full_notm}} service instance you created to the  project. For details, including videos and tutorials, see [IBM Watson Learning ](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started/).
After you have added the {{site.data.keyword.iae_full_notm}} service to the project, you can select to run a notebook on the service. For details on how to run code in a notebook, see [Code and run notebooks](https://dataplatform.ibm.com/docs/content/analyze-data/code-run-notebooks.html?audience=wdp&context=analytics).

## Can I use Kafka for data ingestion?
{: #kafka-4-data-ingestion}
{: faq}

IBM Message Hub, an {{site.data.keyword.Bluemix_notm}} service is based on Apache Kafka. It can be used to ingest data to an object store. This data can then be analyzed on an {{site.data.keyword.iae_full_notm}} cluster. Message Hub can also integrate with Spark on the {{site.data.keyword.iae_full_notm}} cluster to bring data directly to the cluster.

## Can I set ACID properties for Hive in {{site.data.keyword.iae_full_notm}}?
{: #acid-4-hive}
{: faq}

Hive is not configured to support concurrency. Although you can change the Hive configuration on {{site.data.keyword.iae_full_notm}} clusters, it is your responsibility that the cluster functions correctly after you have made any such changes.

## More FAQs

- [General FAQs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-general-faqs)
- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-faqs-architecture)
- [FAQs about {{site.data.keyword.iae_full_notm}} operations](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-operations-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} security](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-security-faqs)
