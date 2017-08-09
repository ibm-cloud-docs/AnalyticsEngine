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

From the [service credentials](./Retrieve-service-credentials-and-service-end-points.html#sample-response) that you created in the previous step, look for the SSH connection string under `service_endpoints` json. It would look something similar to the following:

```
"ssh": "ssh clsadmin@XXXXX-mn002.bi.services.us-south.bluemix.net",
```
SSH to the cluster using the command given above. When prompted, enter the `password` retrieved from the [service key json](./Retrieve-service-credentials-and-service-end-points.html#sample-response).
