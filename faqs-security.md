---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}


# Security FAQs

## What type of encryption is supported?
{: faq}

Hadoop transparent data encryption is automatically enabled for each cluster. The cluster comes with a predefined HDFS encryption zone, which is identified by the HDFS path `/securedir`. Files
that are placed in the encryption zone are automatically  encrypted. The files are automatically decrypted when they are accessed through various Hadoop client applications, such as HDFS
shell commands, WebHDFS APIs, and the Ambari file browser. More information is available in the [documentation](https://{DomainName}/docs/services/AnalyticsEngine/Upload-files-to-HDFS.html).

All data on Cloud Object Storage is encrypted at-rest. You can use a private, encrypted endpoint available from Cloud Object Storage to  transfer data between Cloud Object Storage and {{site.data.keyword.iae_full_notm}} clusters. Any data that passes over the public facing ports (8443,22 and 9443) is encrypted.

## Which ports are open on the public interface on the cluster?
{: faq}

The following ports are open on the public interface on the
cluster:

- Port 8443 Knox
- Port 22 SSH
- Port 9443 Ambari

## More FAQs

- [General FAQs](/docs/services/AnalyticsEngine/faqs-general.html)
- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/services/AnalyticsEngine/faqs-architecture.html)
- [FAQs about {{site.data.keyword.iae_full_notm}} operations](/docs/services/AnalyticsEngine/faqs-operations.html)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/services/AnalyticsEngine/faqs-integration.html)
