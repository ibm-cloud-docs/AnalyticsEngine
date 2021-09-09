---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:pre: .pre}

# Resizing clusters
{: #resize-clusters}

You can scale up your cluster capacity or resources by adding more compute and task nodes. When fewer resources are needed, you can scale down again by removing task nodes. However, you can't scale  down compute nodes. This means that you should add compute nodes only if you absolutely need them. More nodes mean better performance but also increased cost.

You should not scale down by removing task nodes while applications are running because this can lead to application retries or failures.

Task nodes are only supported on clusters created on or after March 22, 2021.
{: note}

To resize your {{site.data.keyword.iae_full_notm}} cluster, you must have the following [user permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).

You can resize the cluster by using one of the following modes:
* [The Cluster Management user interface](#resize-cluster-by-cluster-management-user-interface)
* [Issuing a REST API call](#resize-cluster-using-the-rest-api)

## Resizing clusters by using the Cluster Management user interface
{: #resize-cluster-by-cluster-management-user-interface}

To resize a cluster:
1. On {{site.data.keyword.Bluemix_notm}} console, switch to the organization and space where your service instance was created.
1. Click the service instance tile to access the service dashboard.
1. On the right of the page, click **Manage**. The cluster management page shows you the number of compute nodes in your cluster.
1. Add compute nodes by clicking `+` next to the number of compute nodes.
1. Add task nodes by clicking `+` next to the number of task nodes.
1. Remove task nodes by clicking `-` next to the number of task nodes.
1. Click **Save**.
1. Wait for the clusters to be resized (the time depends on the number of nodes you added), and then refresh the page to verify that your resize request was handled successfully.

  The Nodes section of the cluster management page shows a list of all nodes of the cluster. You can identify the newly added nodes by looking at the creation time shown in the **Nodes** section.  

## Resizing clusters using the REST API
{: #resize-cluster-using-the-rest-api}

To resize cluster using the REST API, you must meet the following prerequisites:
- You must have Editor access to the service instance. Reach out to your {{site.data.keyword.Bluemix_notm}} account owner, if you do not have sufficient permissions. For more details refer to [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).
- The API call to resize the cluster requires your IAM bearer token. To obtain the token, see [Create a token using the IBM Cloud REST API](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).

Examples of resizing a cluster:

- To resize the cluster by one compute node, enter:  
    ```
    curl -i -X POST  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/resize -H 'Authorization: Bearer <user's IAM token>' -d '{"compute_nodes_count":2}' -H "Content-Type:application/json"
    ```

    For the parameter `compute_nodes_count`, you need to pass the expected size of the cluster, after the resize operation. For example, if your cluster currently has one compute node and you want to add two more nodes to it, then the value for `compute_nodes_count` parameter should be 3.
- To resize the cluster to 5 task nodes, enter:
    ```
    curl -i -X POST  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/resize -H 'Authorization: Bearer <user's IAM token>' -d '{"task_nodes_count":5}' -H "Content-Type:application/json"
    ```

    The resize API expects the absolute number of compute or task that you expect in the cluster and will scale up or down based on the given value.

    If the cluster has 12 task nodes for example, the API call will remove 7 task nodes from the cluster, bringing down the number of task nodes to 5.

    Use these endpoints to supported regions:
    - US South: `https://api.us-south.ae.cloud.ibm.com/`
    - US East: `(https://api.us-east.ae.cloud.ibm.com`
    - United Kingdom: `https://api.eu-gb.ae.cloud.ibm.com`
    - Germany: `https://api.eu-de.ae.cloud.ibm.com`
    - Japan: `https://api.jp-tok.ae.cloud.ibm.com`
    - Australia: `https://api.au-syd.ae.cloud.ibm.com`

    You can scale up both compute and task nodes. You can scale down task nodes. However, you cannot scale down compute nodes.
    {: note}
