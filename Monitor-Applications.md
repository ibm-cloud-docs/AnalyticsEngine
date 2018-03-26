---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Monitoring applications

On the {{site.data.keyword.iae_full_notm}} cluster you can use YARN, Spark and Spark History Server web interfaces to monitor running and completed applications.

You can launch YARN's resource manager web interface by opening the Ambari console and navigating to YARN > Quick Links > ResourceManager UI. Use your cluster user credentials when prompted for a user id and password.

YARN ResourceManager UI lists all applications on the `All Applications` page where,

* successfully completed applications show with the state `FINISHED`
* running applications such as currently active Notebooks/interactive sessions show with the state `RUNNING`
* applications that have been accepted by YARN for execution but are waiting to be allocated resources to run them show with the state `ACCEPTED`

The application link from the `All Applications` page takes you to the `Application Overview` page. The `Application Overview` page displays a tracking URL to the Live Spark UI for a running application or the Spark History Server UI for an application that has ended. Both the live Spark UI and the Spark History Server UI allow you to inspect the Spark Jobs, Executors, Storage, SQL, associated logs and other details related to the application.

For a running application, YARN's ResourceManager UI allows you to drill down to the allocated Containers, their associated stderr and stdout logs and cluster node details.
