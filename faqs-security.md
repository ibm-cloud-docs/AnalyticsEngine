---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-05"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}


# Security FAQs

<ul>
<li>[What type of encryption is supported?](#what-type-of-encryption-is-supported-)</li>
<li>[Which ports are open on the public interface on the  cluster?](#which-ports-are-open-on-the-public-interface-on-the-cluster-)</li>
</ul>

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
