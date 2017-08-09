---

copyright:
  years: 2017
lastupdated: "2017-08-04"

---

<!-- Attribute Definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}


# Administer cluster using Ambari console

You can use the Ambari console UI for cluster administration.

The Ambari console URL is available as part of the `ambari_console` property of the [service credentials and end point](./Retrieve-service-credentials-and-service-end-points.html). Use the values of `user` and `password` fields of the [service end point json](./Retrieve-service-credentials-and-service-end-points.html#sample-response) to log on to the Ambari console.

The cluster user `clsadmin` is granted `Service Administrator` privilege, which provides access to perform the following tasks:

* View and modify service configurations.
* Start and stop services.
* View service status and health alerts.
