---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-26"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Connecting using SSH
{: #connect-SSH}

{{site.data.keyword.iae_full_notm}} supports password based SSH connectivity.

In the [service credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints) that you created, look for the SSH connection string under `service_endpoints` in the JSON output. For example, enter the following SSH command to access the cluster:

```
"ssh": "ssh clsadmin@XXXXX-mn003.<changeme>.ae.appdomain.cloud"
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

See [Retrieving cluster credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-cluster-credentials) to get the cluster password for which you are prompted before you can  access the cluster.
