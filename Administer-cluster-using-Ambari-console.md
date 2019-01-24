---

copyright:
  years: 2017,2018
lastupdated: "2018-10-18"

---

<!-- Attribute Definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}


# Administering clusters by using the Ambari console

You can use the Ambari console UI for cluster administration.

The URL to the Ambari console is made available to you as part of the `ambari_console` property of the [service credentials and end point](/docs/services/AnalyticsEngine/Retrieve-service-credentials-and-service-end-points.html). Use the values in the `user` and `password` fields in the sample response of the service end point JSON to log on to the Ambari console.

The `clsadmin` user is granted `Service Administrator` privileges, which provides access to perform the following actions:

* View and modify service configurations.
* Start and stop services
* View service status and health alerts.
