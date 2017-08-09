---

copyright:
  years: 2017
lastupdated: "2017-07-23"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# JNBG Service Host

The JNBG service is only accessible via the published service endpoints of the cluster.

The host on which the service runs is currently not published in the cluster service endpoints. If you need to SSH to this host, such as to access the kernel gateway or kernel logs, follow these steps:

**To SSH to a host**

* Obtain the SSH endpoint from your cluster service key json. When you SSH to this endpoint, it leads to the host where JNBG is running.
