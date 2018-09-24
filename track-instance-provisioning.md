---

copyright:
  years: 2017,2018
lastupdated: "2018-09-24"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Tracking the status of the cluster provisioning

The following diagram illustrates the various states of a cluster during cluster creation, cluster resizing, and cluster deletion.

![Shows the various states during cluster  provisioning.](images/cluster-states.png)

You can track the status of your cluster provisioning by using the following REST API:

```curl -i -X GET   https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/state -H 'Authorization: Bearer <user's IAM access token>'
```  

**Note:** For the United Kingdom region, use the endpoint `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use the endpoint `https://api.eu-de.ae.cloud.ibm.com`.

To retrieve the service instance ID, see [Retrieviving the service instance ID](./retrieve-service-instance-id.html). For the IAM access token, see [Retrieving IAM access tokens](./Retrieve-IAM-access-token.html).

Expected response:

The overall cluster state is returned in JSON format, for example, ` {"state":"Active"}`
