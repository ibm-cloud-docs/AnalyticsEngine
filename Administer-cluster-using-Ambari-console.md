---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}


# Administering clusters by using the Ambari console
{: #adm-ambari}

You can use the Ambari console UI for cluster administration. To use the Ambari console UI, you must have the following [user permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).

The URL to the Ambari console is made available to you as part of the `ambari_console` property of the [service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints). See [Retrieving cluster credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-cluster-credentials) for how to get the credentials to log on to the Ambari console.

The `clsadmin` user is granted `Service Administrator` privileges, which provides access to perform the following actions:

* View and modify service configurations.
* Start and stop services
* View service status and health alerts.
