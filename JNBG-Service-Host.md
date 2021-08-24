---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# JNBG Service Host
{: #JNBG-host}

The JNBG service is only accessible via the published service endpoints of the cluster.

However, if you need to access the host on which JNBG runs, for example to access the kernel gateway logs or the kernel driver logs, you need to connect to the cluster using SSH. See [Connecting using SSH](/docs/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH) to SSH to the cluster. When you SSH to this endpoint, it leads to the same host where the JNBG service is running.
