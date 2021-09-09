---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-31"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}


# Configuring log aggregation
{: #log-aggregation-serverless}

{{site.data.keyword.iae_full_notm}} supports aggregating cluster logs to a centralized log server that you own. Currently {{site.data.keyword.la_short}} is the only supported log server that you can aggregate logs to.

You can collect the logs for the following components in an {{site.data.keyword.iae_full_notm}} instance:

- {{site.data.keyword.iae_full_notm}} daemon logs for Spark on the management and data nodes
- Yarn application job logs

## Aggregation operations

There are different ways for you to aggregate cluster logs.
You can:
- [Configure aggregating logs](#configuring-log-aggregation) for a chosen combination of cluster nodes and components. For example, you can:

   - Configure to collect only Yarn application logs
   - Configure to collect only daemon logs from data nodes
   - Configure to collect the logs from all nodes

-	[Reconfigure log aggregation](#reconfiguring-log-aggregation) by changing the configuration. For example, you can:

   - Change the {{site.data.keyword.la_short}} destination server
   - Update the ingestion key
   - Change the nodes and components from where you want to aggregate logs.

- [Retrieve the status](#retrieving-the-status-of-the-log-configuration) of the log configuration.
-	[Delete the log aggregation configuration](#deleting-the-log-configuration). Deleting the configuration stops all log collecting to the centralized log server.

**NOTE**: If log aggregation is configured for data nodes, the configuration is automatically applied on all newly added nodes as well.

## Prerequisites
{: #log-aggregation-prereqs}

The following prerequisites must be met before you can begin collecting cluster logs to a centralized server:

- You must have an existing {{site.data.keyword.iae_full_notm}} service instance. Presently, REST API is the only mode with which you can configure log aggregation.
- You must create an {{site.data.keyword.la_full_notm}} service instance. To create an instance in {{site.data.keyword.Bluemix_short}}, see [{{site.data.keyword.la_full_notm}}
](https://cloud.ibm.com/observe/logging){: external}. For details on monitoring and managing log data with {{site.data.keyword.la_full_notm}}, see [provisioning a service instance](/docs/log-analysis?topic=log-analysis-provision){: external}.
- You must have access to the {{site.data.keyword.la_short}} ingestion key. See [Getting the ingestion key](/docs/log-analysis?topic=log-analysis-ingestion_key){: external}.
- You must have the following IAM access permissions (roles) to the {{site.data.keyword.iae_full_notm}} service instance and the resource group. Two types of roles exist:

    -	**Platform management role**: here you must have viewer access or higher to the resource group that contains {{site.data.keyword.iae_full_notm}}.
    - **Service access role**: here you must have manager access or higher to the {{site.data.keyword.iae_full_notm}} service instance.

 See [Granting permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions){: external}.  
- You need your IAM access token. See [Retrieving the IAM access token](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token){: external}.
-	You need the `cluster_management.api_url` for the service endpoints of your {{site.data.keyword.iae_full_notm}} service instance. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints){: external}.  
- When you configure  {{site.data.keyword.iae_full_notm}} to work with the {{site.data.keyword.la_short}} instance, you can select to connect to the **private** endpoints of the instance. We encourage you to use  private endpoints as this increases performance and is more cost effective. See [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration){: external}.

## Configuring log aggregation

You configure log aggregation by invoking the PUT operation on the  `log_config` endpoint of the {{site.data.keyword.iae_full_notm}}  cluster management API.

```
curl -X  PUT  \
https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/log_config
\ -H 'authorization: Bearer  <user's IAM token>'
\ -H "Content-Type: application/json"
\ -d @log-config.json
```

This is an example of what the `log-config.json` could look like:
```
{
    "log_specs": [{
        "node_type": "management",
        "components": ["knox", "ambari-server"]
    }, {
        "node_type": "data",
        "components": ["yarn-apps"]
    }],
    "log_server": {
        "type": "logdna",
        "credential": "xxxxxxxxxxxxxxxx",
        "api_host": "api.us-south.logging.cloud.ibm.com",
        "log_host": "logs.us-south.logging.cloud.ibm.com"
    }
}
```
For the `api_host` and `log_host` input parameters, use the region specific endpoints of your {{site.data.keyword.la_short}} instance. Supported regions of {{site.data.keyword.la_short}} service instances are:
- `us-south` (for Dallas)
- `eu-gb` (for London)
- `eu-de` (for Frankfurt)
- `jp-tok` (for Tokyo)

You can use the following component names:

**Management node components**  
- `ambari-server`
- `hadoop-mapreduce`
- `hadoop-yarn`
- `hbase`
- `hdfs`
- `hdfs-audit`
- `hive`
- `jnbg`
- `knox`
- `knox-audit`
- `livy2`
- `spark2`

**Data node components**
- `hadoop-mapreduce`
- `hadoop-yarn`
- `hdfs`
- `hdfs-audit`
- `spark2`
- `yarn-apps`

## Reconfiguring log aggregation

You can update the log configuration by invoking the same REST API you used for configuring log aggregation. However, note that when you reconfigure, the existing configuration is overwritten. For example, if you invoked the configure API on both the management and data nodes, and then you reconfigure the API for the data node only, the management node’s log configuration is removed.

## Retrieving the status of the log configuration

You retrieve the status and details of the log configuration for your cluster by invoking the GET API.

```
curl -X  GET  \
https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/log_config
\ -H 'authorization: Bearer  <user's IAM token>'
```
This is a sample response:
```
{
    "log_specs": [{
        "node_type": "management",
        "components": ["knox", "ambari-server"]
    }, {
        "node_type": "data",
        "components": ["yarn-apps"]
    }],
    "log_server": {
        "type": "logdna",
        "credential": "*****",
        "api_host": "api.us-south.logging.cloud.ibm.com",
        "log_host": "logs.us-south.logging.cloud.ibm.com",
        "owner": "user"
    },
    "log_config_status": [{
        "node_type": "management",
        "node_id": "mn001"
        "action": "configure",
        "status": "Completed"
    }, {
        "node_type": "data",
        "node_id": "dn001"
        "action": "configure",
        "status": "Completed"
    }, {
        "node_type": "data",
        "node_id": "dn002"
        "action": "configure",
        "status": "Failed"
    }]
}
```
## Deleting the log configuration

You delete the log configuration by invoking the DELETE API. This operation stops sending logs to the centralized log server.

```  
curl -X  DELETE  \
https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/log_config
\ -H 'authorization: Bearer  <user's IAM token>'
```
