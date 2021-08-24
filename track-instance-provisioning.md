---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Tracking the status of the cluster provisioning
{: #track-provisioning}

The following diagram illustrates the various states of a cluster during cluster creation, cluster resizing, and cluster deletion. After node preparation and successful deployment of the cluster, the cluster state is active. In active state, the cluster can be customized and resized. While the cluster is active, all maintenance activities can be carried out.

![Shows the various states during cluster provisioning](images/cluster-states-new.svg)

You can track the status of your cluster provisioning by using the following REST API:

```
curl -i -X GET   https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/state -H 'Authorization: Bearer <user's IAM access token>'
```  

**Note:** For the United Kingdom region, use the endpoint `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use  `https://api.eu-de.ae.cloud.ibm.com` and for Japan `https://api.jp-tok.ae.cloud.ibm.com`.

To retrieve the service instance ID, see [Retrieving the service instance ID](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-service-id). For the IAM access token, see [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).

Expected response:

The overall cluster state is returned in JSON format, for example, ` {"state":"Active"}`
