---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

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

The URL to the Ambari console is made available to you as part of the `ambari_console` property of the [service credentials and end point](./Retrieve-service-credentials-and-service-end-points.html). Use the values in the `user` and `password` fields of the [service end point json](./Retrieve-service-credentials-and-service-end-points.html#viewing-the-service-key) to log on to the Ambari console.

The `iaeadmin` user is granted `Service Administrator` privileges, which provides access to perform the following actions:

* View and modify service configurations.
* Start and stop services
* View service status and health alerts.

The Ambari console URL is available as part of the `ambari_console` property of the [service credentials and end point](./Retrieve-service-credentials-and-service-end-points.html). Use the values of `user` and `password` fields of the [service end point json](./Retrieve-service-credentials-and-service-end-points.html#sample-response) to log on to the Ambari console.


The cluster user `clsadmin` is granted `Service Administrator` privilege, which provides access to perform the following tasks:

* View and modify service configurations.
* Start and stop services.
* View service status and health alerts.
