---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-24"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Customizing a cluster
{: #cust-cluster}

Sometimes you might have to customize a cluster specifically to suit your needs, over and above what is provisioned on a default basis.

For example, you might want to install custom analytics third-party libraries or you might want to fine-tune some cluster configurations, for example, the Hadoop MapReduce heap size.

These customizations might need to be applied and executed every time a new cluster is created or be executed iteratively on an existing cluster as needed. To this end, a shell script with all the required customizations can be placed at some source, such as the HTTP or Cloud Object Storage location, and given as input to be executed to customize the cluster.

The customization feature can be invoked in two ways, namely as:
- **Bootstrap customization**: specified at the time the cluster is created
- **Adhoc customization**: run on a need basis after the cluster is
created

To customize an {{site.data.keyword.iae_full_notm}} cluster, you must have the following [user permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).

## Bootstrap customization
In this method, the customization script can be specified as part of the input JSON to the create cluster command as is shown in the examples later. In this case, the script is executed as a final step after the cluster is created. Even if the customization fails the cluster is still available for use.

**Note**: Bootstrap customization is not recommended for customizing Ambari components like Hadoop or Spark. Instead you should specify  [advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options).

**Rerunning a bootstrap customization script**: If the customization fails due to any reason, the same action can be rerun at a later point in time on the required targets.

Consider the following aspects:
- The bootstrap customization script is executed on all nodes of the cluster including the management and compute nodes.
-	The bootstrap customization action specified during cluster creation is automatically executed on any new compute node added during the cluster resize operation.
- Currently, bootstrap customization is possible only using the Cloud Foundry CLI or the Cloud Foundry REST API modes for creating a cluster. That is, it cannot be specified via the GUI.

## Adhoc customization
If you do not want, or forgot to specify the customization options during cluster creation, you can still customize your cluster by using the adhoc customization anytime you want.
The cluster must be in an active state to enable customization and you need to specify the target for execution of the script.

## Differences between bootstrap and adhoc customization
The main differences between these customization methods is shown in the following table:

| Bootstrap customization | Adhoc customization |
|-------------------|---------------------------|
| Defined during cluster creation | Defined and executed on an active cluster |
|By default executed on all nodes of the cluster |Need to specify target of execution |
|Can be rerun later, if needed, on a given target list. (There is no need to specify the script details) |Can be run as often as is needed by specifying the script location details|
|Automatically run on newly-added nodes | Not run automatically on nodes added to the cluster |

## Location of the customization script
You can add a customization script to the following sources:
*	HTTP with or without basic authentication
*	HTTPS with or without basic authentication
*	{{site.data.keyword.cos_full_notm}}

Examples for each type are given in the following sections.

## Specifying the target for a running customization

As mentioned before, in the case of boostrap customization, the script runs on all nodes.
You need to specify a target only when you run:
 - An adhoc customization
 - Or when you need to rerun a bootstrap customization script

The target can be one of the following four types
  - `all`: runs the customization on all nodes of the cluster, including management and compute nodes
  - `data`: runs the customization on all compute nodes
  - `management2` or `management3` (the `management-slave1` or `management-slave2` nodes are deprecated): runs on the management2 or management3 node as specified. You may need this for configuring Ambari parameters as give in the example section.
  - A comma separated list of fully qualified node names: This runs on the given list of nodes only.

If the target is multiple nodes, the customization scripts are executed in parallel.

**Important**: You cannot name the `management1` node (the  master management node is deprecated) as a target.

## Predefined environment variables available for use in the customization script
The following predefined environment variables are available that can be used in the customization script:
`AMBARI_HOST`, `AMBARI_PORT`, `AMBARI_USER`, `CLUSTER_NAME`, and `NODE_TYP`. `NODE_TYP` can take the value `management2` or `management3`.  

Note that the environment variable `NODE_TYPE` and its values  `data`, `management-slave1` and  `management-slave2` are deprecated. Start using the new environment variable `NODE_TYP`.

## Package Admin tool
The customization script is always executed as `cluster user`. However, the default rights of the cluster user do not allow all operations, for example, a YUM install. In such cases, you need to use the `package-admin` tool.
The `package-admin` tool is a special utility tool available for use in the {{site.data.keyword.iae_full_notm}} cluster, which you can use to install or remove operating system packages. You can use it to install or remove YUM packages from supported repos (only centOS base and updates).

`sudo package-admin -c [install | remove] -p [package name]`

This is something you can use in the customization script or even directly on any of the cluster nodes, after you [SSH](/docs/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH) to it.
Note the use of `sudo` in order to execute the utility.

## What can you customize?

You can customize:
- The installation or removal of operating system packages
- The installation of analytics Python and R libraries
- Ambari configuration parameters

The customization script will run as long as it contains code that the user of the cluster is authorized to execute. It cannot execute code that requires root access. For example, it cannot execute code such as opening ports or changing IP table rules.

## Tracking the status of the customization
This is a three step process. First you need to get the customization request ID for your instance and then invoke a second API to get the status of that particular ID. From the second invocation, you will get the location details of the customization logs for each target node. Finally, if you need to look at the log details, you will need to [SSH](/docs/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH) to the specific node.

### Step 1 - Getting all customization requests for the given instance ID

Use the following cluster management REST API to get the customization requests for the given instance ID:

```
curl -X GET  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests -H 'Authorization: Bearer <user's IAM access token>'
```

For the United Kingdom region, use the endpoint `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use `https://api.eu-de.ae.cloud.ibm.com`. For Tokyo, use `https://api.jp-tok.ae.cloud.ibm.com`.

**Expected response:** The customization requests for the given service instance ID are returned in JSON format. For example:

`[{"id":"37"},{"id":"38"}]`

### Step 2 - Getting the details of a specific customization request

Use the following cluster management REST API to get the details of a specific customization request:

```
curl -X GET  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests/<request_id> -H 'Authorization: Bearer <user's IAM access token>'
```

**Expected response:** The customization request details are returned in JSON format.
- The `run_status` is the overall status of execution of the script. It can be `InProgress` or `Failed` or `Completed`. If for instance, the script could not be executed because an invalid location was specified, the run_status would be `Failed`
- The `overall_status` of customization is a summary of the customization status on the individual nodes. It can be in `progress`, `failed` or `success`. If all nodes are successful, the `overall_status` would be `success`, otherwise if one or more failed, it would be `fail`.
- The individual status for each node's customization is given in the `status` attribute of each node. It could be `Customizing`,  `CustomizeSuccess` or  `CustomizeFailed`. For instance, if a wrong environment variable was specified for a node, then the customization for that node could have failed.

If there is a failure, you may need to rerun the customization for that target or once again for all targets depending on at which point the customization failed.

For example:

```
{
	"id": "37",
	"run_status": "Completed",
	"run_details": {
		"overall_status": "success",
		"details": [{
			"node_name": "chs-fpw-933-mn003.<region>.ae.appdomain.cloud",
			"node_type": "management-slave2",
			"start_time": "2017-06-06 11:46:35.519000",
			"end_time": "2017-06-06 11:47:46.687000",
			"time_taken": "71 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-mn003.<region>.ae.appdomain.cloud_37.log"
		}, {
			"node_name": "chs-fpw-933-mn002.<region>.ae.appdomain.cloud",
			"node_type": "management-slave1",
			"start_time": "2017-06-06 11:46:36.190000",
			"end_time": "2017-06-06 11:47:46.864000",
			"time_taken": "70 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-mn002.<region>.ae.appdomain.cloud_37.log"
		}, {
			"node_name": "chs-fpw-933-dn001.<region>.ae.appdomain.cloud",
			"node_type": "data",
			"start_time": "2017-06-06 11:46:36.693000",
			"end_time": "2017-06-06 11:47:47.271000",
			"time_taken": "70 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-dn001.<region>.ae.appdomain.cloud_37.log"
		}]
	}
}
```

where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).

### Step 3 - Getting the details of a specific node's customization

You can retrieve the log file in `log_file` by using [`ssh/scp`](/docs/AnalyticsEngine?topic=AnalyticsEngine-connect-SSH) to the corresponding node. This log captures the output of script execution, including the `echo` statements. If the script could not be executed due to a bad location or bad credentials specified, you will see details of the error in the log. The following example shows the log for such a case.

```
[clsadmin@chs-mwb-189-mn003 ~]$ cat /var/log/chs-mwb-189-mn003.<region>.ae.appdomain.cloud_28.log
Error while downloading customization script, ResponseCode: 0, Please verify source_props and  script_path properties in bootstrap action
```

where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).
## Customization examples
For examples of how to customize a cluster, see [Examples of customizations](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-examples).
