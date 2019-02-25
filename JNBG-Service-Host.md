---

copyright:
  years: 2017, 2019
lastupdated: "2017-11-02"

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

However, if you need to access the host on which JNBG runs, for example to access the kernel gateway logs or the kernel driver logs, follow the steps [here](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH) to SSH to the cluster. When you SSH to this endpoint, it leads to the same host where the JNBG service is running.
