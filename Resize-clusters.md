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

# Resizing clusters
You can scale up your cluster by adding more compute nodes. More nodes mean better performance, but also increased cost. For efficiency, add all the nodes that you need at once rather than one at a time.

You can resize the cluster using one of the following modes:
* Cluster Management user interface
* REST API

## Resizing clusters using the Cluster Management user interface

**To resize a cluster**

1. On {{site.data.keyword.Bluemix_notm}} console, switch to the organization and space where your service instance was created.
2. Click the service instance tile to access the service dashboard.
3. On the right hand side of the page, click **Manage**. The cluster management page shows you the number of compute nodes in your cluster.
4. Click `+` next to the number of compute nodes and click **Save**.
5. Wait for a few seconds for the clusters to be resized, and then refresh the page to verify that your resize request was handled successfully.
  The Nodes section of the cluster management page shows a list of all nodes of the cluster. You can identify the newly added nodes from the creation time shown in the **Nodes** section.  

## Resizing clusters using the REST API

**Pre-requisites**:
* To resize a cluster, you should have Editor access to the service instance. Reach out to your {{site.data.keyword.Bluemix_notm}} account owner, if you do not have sufficient permissions. For more details refer to [Retrieving IAM access tokens](./Retrieving-IAM-access-tokens.html).
* The API call to resize the cluster requires your IAM bearer token. To obtain the token, follow these [steps](./Retrieve-IAM-access-token.html).

**To resize a cluster**

* Enter the following command. For example, to increase the cluster by one node:  
```
curl -i -X POST https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_guid>/resize -H 'Authorization: Bearer <user's IAM token>' -d '{"compute_nodes_count":2}' -H "Content-Type:application/json"
```

For the parameter `compute_nodes_count`, you need to pass the expected size of the cluster, after the resize operation. For example, if your cluster current has one compute node and you want to add two more nodes to it, then the value for `compute_nodes_count` parameter should be 3.

**Restriction**: Currently, only the scale up operation is supported. Removing nodes from a cluster is not supported. You can also have a maximum of three compute nodes.
