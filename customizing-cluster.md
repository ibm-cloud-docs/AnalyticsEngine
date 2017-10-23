---

copyright:
  years: 2017
lastupdated: "2017-09-25"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Customizing a cluster

When you create a cluster, you can customize the cluster. Custom actions include installing additional analytics libraries or updating existing cluster configuration parameters. There are two customization methods:

- Bootstrap customization
- Adhoc customization

The main differences between these customization methods is shown in the following table:  

<table>
<tr><th>Bootstrap customization</th><th>Adhoc customization</th></tr>
<tr><td>Defined during cluster creation</td><td>Defined and executed on an active cluster</td></tr>
<tr><td>Can be rerun later on a given target list</td><td>The action is not persisted hence cannot rerun </td></tr>
<tr><td>Automatically run on with newly-added nodes</td><td>Not run on nodes added to the cluster</td></tr>
</table>

Customization options specified during cluster creation can be [rerun](#rerunning-a-bootstrap-customization-script-registered-during-cluster-creation) at a later point in time, for example, after a custom action fails to run. However, if you do not specify customization options during cluster creation, you can still customize your cluster by using the [adhoc customization](#running-an-adhoc-customization-script). The bootstrap customization action specified during cluster creation is automatically executed on any new node added during the cluster resize operation.

**Note:** Presently, bootstrap customization is possible using the cf CLI or the cf REST API modes for creating a cluster.

You can [track the status of customization through a REST API](#getting-cluster-status) as described in the sections below.

## Creating a cluster with customization in JSON format

Review the example for creating a cluster with customization in JSON format.
```
cf create-service IBMAnalyticsEngine Standard <service instance name> -c <cluster parameters as json string or path to cluster parameters json file>
```
{: codeblock}

A sample parameters json is given below:
```
{
	"num_compute_nodes": 1,
	"hardware_config": "Standard",
	"software_package": "ae-1.0-spark",
	"customization": [{
		"name": "action1",
		"type": "bootstrap",
		"script": {
			"source_type": "http",
			"script_path": "http://path/to/your/script"
			},
		"script_params": []
        }]
}
```
{: codeblock}

Where:
* `name` is the name of your customization action.
* `type` is either `bootstrap` or `teardown`. Currently only `bootstrap` is supported.

**Note:** Presently, only one custom action can be specified in the `customization` array.

## Creating a cluster with customization by using the Cloud Foundry API

Review the example for creating a cluster with customization by using the Cloud Foundry API.

```
  curl --request POST \
  --url 'https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true' \
  --header 'accept: application/json' \
  --header 'authorization: <User's UAA bearer token>' \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/json' \
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"febf38af-bb11-4d55-8732-49a9b67a480f", "parameters": { "hardware_config":"Standard", "num_compute_nodes":1, "software_package":"ae-1.0-spark", "customization":[<customization-details>]}}'
```
{: codeblock}

**Notes:**
* If you need to find your UAA bearer token, see [Obtaining the Cloud Foundry UAA bearer token](./provisioning.html#Obtaining-the-Cloud-Foundry-UAA-bearer-token).
* If you need to find your space GUIDs, see [Obtaining the space GUID](./provisioning.html#obtaining-the-space-guid).

## Customization options

You can customize the following options:

### Operating system packages

You can install or remove operating system packages by using the package-admin tool. The IBM Analytics Engine cluster comes bundled with a utility named `package-admin` that you can use to install or remove yum packages.
```
 sudo package-admin -c [install | remove] -p [package name]
```
### Install Python and R libraries

There are two versions of Anaconda installed on all nodes:

* Anaconda with Python 2.7 at `/home/common/conda/anaconda2`
* Anaconda with Python 3.5 at `/home/common/conda/anaconda3`

In your script, use commands like:
```
/home/common/conda/anaconda[2|3]/bin/pip install [python or R packages]
```   
### Change Ambari configurations

Ambari configurations are only applicable for the master management node. To ensure that these commands run only on the master management node, add the following check to your script:

`if [ "x$NODE_TYPE" == "xmaster-management" ]`

For example:
```
if [ "x$NODE_TYPE" == "xmaster-management" ]
then
    echo "Updating ambari config properties"
    #change mapreduce.map.memory to 8192mb
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME mapred-site "mapreduce.map.memory.mb" "8192"
    # stop MAPREDUCE2 service
    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Stop MAPREDUCE2"}, "ServiceInfo": {"state": "INSTALLED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services/MAPREDUCE2
    sleep 60
    # start MAPREDUCE2 service
    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Start MAPREDUCE2"}, "ServiceInfo": {"state": "STARTED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services/MAPREDUCE2
fi
```
### Configure either Swift or COS/S3 Object Storage as a data source for Hadoop/Spark

For details see [Configuring clusters to work with IBM COS S3 object stores](./configure-COS-S3-and-Swift-object-storage.html).

## Location of the customization script

You can add a customization script to the following sources:
* Http with or without basic authentication
*	Https
*	Bluemix object storage
*	Softlayer swift
*	Softlayer COS S3

The following sections provide sample actions for the various sources.

### HTTP (with or without basic authentication)
```       
"customization":[ {
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "http",
        "script_path": "http://host:port/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
  }]
```
### HTTPS
```      
"customization":[{
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "https",
        "source_props": {
             "username": "user",
             "password": "pwd"
         },
         "script_path": "https://host:port/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
  }]
```
### Bluemix object store
```
"customization":[ {
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "BluemixSwift",
        "source_props": {
           "auth_url": "https://identity.open.softlayer.com",
            "user_id": "xxxxxxxx",
           "password": "yyyyyyyyyyj",
           "project_id": "zzzzzzzzz",
           "region": "dallas"
         },
         "script_path": "/myContainer/myFolder/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
  }]
```
For more detail on how to store your script and artifacts in Bluemix object store and use the same script during customization see the [samples page](./Customization-script-on-Bluemix-Object-Store.html).

### SoftLayer Swift
```
"customization":[ {
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "SoftLayerSwift",
        "source_props": {
           "auth_endpoint": "https://dal05.objectstorage.service.networklayer.com/auth/v1.0/",
           "username": "xxxxxxx",
           "api_key": "yyyyyyy"
         },
         "script_path": "/myContainer/myFolder/bootstrap.sh"
     },
    "script_params": ["arg1", "arg2"]
  }]
```
### Softlayer COS S3
```
"customization":[ {
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "CosS3",
        "source_props": {
            "auth_endpoint": "s3-api.dal-us-geo.objectstorage.service.networklayer.com",
            "access_key_id": "xxxxxxx",
           "secret_access_key": "yyyyyy"
         },
         "script_path": "/myBucket/myFolder/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
  }]
```
## Pre-requisites for all cluster management API calls

To run cluster management REST APIs, you need to pass your IAM access token. To obtain the token, follow these [steps](./Retrieve-IAM-access-token.html).

### Rerunning a bootstrap customization script registered during cluster creation

A persisted customization script is registered during cluster creation and can be rerun. Enter the following command to rerun a persisted customization script:

```
curl -X POST -v "https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests" -d '{"target":"all"}'  -H "Authorization: Bearer <user's IAM access token>" -H "Content-Type: application/json"
```
{: codeblock}


`target` can be:


- `all `: reruns the customization on all nodes


- `master-management`: reruns the customization only on the master management node


- `data`: reruns the customization on all data nodes  

- a comma separated list of fully qualified node names: reruns on the given list of nodes only.

### Running an adhoc customization script

An adhoc customization script can be run after the cluster was created and can only be run once. Enter the following command to run an adhoc customization script:

```
curl -X POST -v "https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests" -d
'{
	"target": "all",
	"custom_actions": [{
		"name": "action1",
		"script": {
			"source_type": "http",
			"script_path": "http://host:port/bootstrap.sh"
		},
		"script_params": ["arg1", "arg2"]
	}]
}'  
-H "Authorization: Bearer <User's IAM access token>" -H "Content-Type: application/json"
```
{: codeblock}


### Getting cluster status

Enter the following cluster management REST API to get cluster status information:

```
curl -i -X GET https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/state -H 'Authorization: Bearer <user's IAM access token>'

```
{: codeblock}

Expected response: The cluster state is returned in JSON format, for example, ` {"state":"Active"}`

### Getting all customization requests for the given instance ID

Enter the following cluster management REST API to get the customization requests for the given instance ID:

```
curl -X GET https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests -H 'Authorization: Bearer <user's IAM access token>'

```
{: codeblock}

Expected response: The customization requests for the given service instance ID are returned in JSON format. For example:

```
[{"id":"37"},{"id":"38"}]
```

### Getting the details of a specific customization request

Enter the following cluster management REST API to get the details of a specific customization request:

```
curl -X GET https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests/<request_id> -H 'Authorization: Bearer <user's IAM access token>'

```
{: codeblock}

Expected response: The customization request details are returned in JSON format. For example:

```
{
	"id": "37",
	"run_status": "Completed",
	"run_details": {
		"overall_status": "success",
		"details": [{
			"node_name": "chs-fpw-933-mn001.bi.services.us-south.bluemix.net",
			"node_type": "master-management",
			"start_time": "2017-06-06 11:46:35.519000",
			"end_time": "2017-06-06 11:47:46.687000",
			"time_taken": "71 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-mn001.bi.services.us-south.bluemix.net_37.log"
		}, {
			"node_name": "chs-fpw-933-mn002.bi.services.us-south.bluemix.net",
			"node_type": "management-slave1",
			"start_time": "2017-06-06 11:46:36.190000",
			"end_time": "2017-06-06 11:47:46.864000",
			"time_taken": "70 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-mn002.bi.services.us-south.bluemix.net_37.log"
		}, {
			"node_name": "chs-fpw-933-dn001.bi.services.us-south.bluemix.net",
			"node_type": "data",
			"start_time": "2017-06-06 11:46:36.693000",
			"end_time": "2017-06-06 11:47:47.271000",
			"time_taken": "70 secs",
			"status": "CustomizeSuccess",
			"log_file": "/var/log/chs-fpw-933-dn001.bi.services.us-south.bluemix.net_37.log"
		}]
	}
}
```
