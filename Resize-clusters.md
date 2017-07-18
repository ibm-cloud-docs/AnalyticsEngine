---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Resizing clusters
You can scale up your cluster by adding more compute nodes. More nodes mean better performance, but also increased cost. For efficiency, add all the nodes that you need at once rather than one at a time.

The API call to resize the cluster requires your IAM bearer token. To obtain the token, follow these [steps](./Retrieve-IAM-access-token.html).

To resize the cluster, enter the following command. For example, to increase the cluster by 1 node:  

```
curl -i -X POST https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_guid>/resize -H 'Authorization: Bearer <user's IAM token>' -d '{"compute_nodes_count":2}' -H "Content-Type:application/json"
```
{: codeblock}

For the parameter `compute_nodes_count`, you need to pass the expected size of the cluster, after the resize operation. For example, if your cluster current has five nodes and you want to add two more nodes to it, then the value for `compute_nodes_count` parameter should be seven.

**Restriction**: Currently, only the scale up operation is supported. Removing nodes from a cluster is not supported.
