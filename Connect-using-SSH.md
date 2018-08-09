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


# Connect using SSH

{{site.data.keyword.iae_full_notm}} supports password based SSH connectivity.

In the [service credentials](./Retrieve-service-credentials-and-service-end-points.html) that you created, look for the SSH connection string under `service_endpoints` in the JSON output. For example, enter the following SSH command to access the cluster:

```
"ssh": "ssh clsadmin@XXXXX-mn003.bi.services.<changeme>.bluemix.net"
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

When prompted, enter the `password` that you can retrieve  from the [service key JSON output](./Retrieve-service-credentials-and-service-end-points.html).
