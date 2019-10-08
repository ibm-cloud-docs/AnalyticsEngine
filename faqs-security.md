---

copyright:
  years: 2017, 2018
lastupdated: "2019-05-15"

subcollection: AnalyticsEngine

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}


# Security FAQs
{: #security-faqs}

## What type of encryption is supported?
{: #supported-encryption}
{: faq}

All data on {{site.data.keyword.cos_full_notm}} is encrypted at-rest. You can use a private, encrypted endpoint available from {{site.data.keyword.cos_full_notm}} to  transfer data between {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.iae_full_notm}} clusters. Any data that passes over the public facing ports (8443,22 and 9443) is encrypted. See details in [Best practices](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-best-practices#cos-encryption).

## Which ports are open on the public interface on the cluster?
{: #open-ports}
{: faq}

The following ports are open on the public interface on the
cluster:

- Port 8443 Knox
- Port 22 SSH
- Port 9443 Ambari

## More FAQs
{: #more-faqs-security}

- [General FAQs](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-general-faqs)
- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-faqs-architecture)
- [FAQs about {{site.data.keyword.iae_full_notm}} operations](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-operations-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-integration-faqs)
