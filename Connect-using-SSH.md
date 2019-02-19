---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-31"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Connect using SSH
{: #connect-SSH}

{{site.data.keyword.iae_full_notm}} supports password based SSH connectivity.

In the [service credentials](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-credentials) that you created, look for the SSH connection string under `service_endpoints` in the JSON output. For example, enter the following SSH command to access the cluster:

```
"ssh": "ssh clsadmin@XXXXX-mn003.<changeme>.ae.appdomain.cloud"
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

When prompted, enter the `password` that you can retrieve  from the [service key JSON output](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-credentials).
