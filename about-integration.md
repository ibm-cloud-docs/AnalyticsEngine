---

copyright:
  years: 2017, 2023
lastupdated: "2023-07-19"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with {{site.data.keyword.lakehouse_short}}
{: #spk_wxd}


## About {{site.data.keyword.lakehouse_short}}
{: #intro-about}

{{site.data.keyword.lakehouse_full}} is a data management solution for collecting, storing, querying, and analyzing all your enterprise data with a single unified data platform. It provides a flexible and reliable platform that is optimized to work on open data formats.
The key features of {{site.data.keyword.lakehouse_full}} include :
- An architecture that fully separates compute, metadata, and storage to offer ultimate flexibility.
- A distributed query engine based on Presto, which is designed to handle modern data formats that are highly elastic and scalable.
-  Data sharing between watsonx.data, {{site.data.keyword.dashdbshort_notm}}, and {{site.data.keyword.netezza_short}} or any other data management solution through common Iceberg table format support, connectors, and a shareable metadata store.

To provision a {{site.data.keyword.lakehouse_short}} instance, see [Getting started with watsonx.data](https://cloud.ibm.com/docs/watsonxdata?topic=watsonxdata-getting-started){: external}.


## Use cases for {{site.data.keyword.iae_short}}
{: #use_case}

You need an {{site.data.keyword.iae_full_notm}} Spark instance to work with {{site.data.keyword.lakehouse_short}} to achieve the following specific use-cases:
- Ingesting large volumes of data into {{site.data.keyword.lakehouse_short}} tables (S3, COS or compatible storage). You can also cleanse and transform data by using Spark procedural code before the ingestion. You can query the data from tables by using the available engines from {{site.data.keyword.lakehouse_short}}.
- Table maintenance operations to enhance {{site.data.keyword.lakehouse_short}} performance of the tables. Using the Iceberg table format, you can use Spark to perform operations such as file compaction, snapshot cleanup, removal of orphan files, schema evolution.
- For complex analytics that are difficult to represent as queries, Spark procedural programming is a suitable solution for data transformation.

To get started with {{site.data.keyword.lakehouse_short}} and {{site.data.keyword.iae_full_notm}} Serverless Spark, see [Provisioning an Analytics Engine instance](https://cloud.ibm.com/docs/watsonxdata?topic=watsonxdata-lh-provisioning-serverless){: external}.
