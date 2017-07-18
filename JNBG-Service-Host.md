---

copyright:
  years: 2017
lastupdated: "2017-07-12"

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

1. Obtain the SSH endpoint from the cluster service endpoint details.
2. SSH to the SSH endpoint which is the JNBG service hostname.

For example, if the SSH hostname from the cluster service endpoints is: `chs-zbh-288-mn003.bi.services.us-south.bluemix.net` and SSH username as `iaeadmin`, the ssh to the JNBG host is:

  ```
  ssh iaeadmin@chs-zbh-288-mn003.bi.services.us-south.bluemix.net
  ```
