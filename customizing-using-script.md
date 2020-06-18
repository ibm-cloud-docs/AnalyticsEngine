---

copyright:
  years: 2017, 2020
lastupdated: "2019-07-31"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Customizing the cluster using a customization script
{: #cust-cluster-script}

To enable an application to connect to {{site.data.keyword.cos_full_notm}}, you must update the {{site.data.keyword.iae_full_notm}} cluster configuration file (core-site.xml file) to include Cloud Object Storage credentials and other required connection values.

One way of doing this is to use a customization script to configure the properties that need to be updated in the core-site.xml file. It uses the Ambari configs.py file to make the required changes.

This [customization sample script](https://github.com/IBM-Cloud/IBM-Analytics-Engine/blob/master/customization-examples/associate-cos.sh) configures the {{site.data.keyword.iae_full_notm}} cluster with HMAC authentication parameters. The example shows the complete source code of the customization script. You can modify the script for IAM authentication style parameters. The sample configuration script restarts the affected services that need to be restarted. These services are started immediately instead of sleeping for a long random interval after the Ambari API was triggered.
