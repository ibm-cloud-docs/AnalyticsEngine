---

copyright:
  years: 2017
lastupdated: "2017-07-20"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Connect using SSH

IBM Analytics Engine supports password based SSH connectivity.

In the [service credentials](./Retrieve-service-credentials-and-service-end-points.html#viewing-the-service-key) that you created, look for the SSH connection string under `service_endpoints` in the JSON output. It would look something similar to the following:

```
"ssh": "ssh clsadmin@XXXXX-mn002.bi.services.us-south.bluemix.net",
```
Enter this SSH command to access the cluster. When prompted, enter the `password` that you can retrieved from the [service key JSON output](./Retrieve-service-credentials-and-service-end-points.html#viewing-the-service-key).
