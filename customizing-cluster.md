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


# Customizing a cluster

When you create a cluster, you can customize the cluster. Custom actions include installing additional analytics libraries or updating existing cluster configuration parameters. There are two customization methods:

- Persisted customization
- Adhoc customization

The main differences between these customization methods is shown in the following table:  

<table>
<tr><th>Persisted customization</th><th>Adhoc customization</th></tr>
<tr><td>Defined during cluster creation</td><td>Defined and executed on an active cluster</td></tr>
<tr><td>Can be rerun later on a given target list</td><td>The action is not persisted hence can not rerun </td></tr>
<tr><td>Automatically executed with newly added nodes(scale-up)</td><td>Not executed with scale-up</td></tr>
</table>

Customization options specified during cluster creation can be [rerun](./customizing-cluster.html/#rerunning-a-persisted-customization-script) at a later point in time, for example, after a custom action fails to run. However, if you do not specify customization options during cluster creation, you can still customize your cluster by using [adhoc customization](./customizing-cluster.html/#running-an-adhoc-customization-script). The customization action specified during cluster creation is automatically executed on any new node added during the cluster resize operation.

You can [track the status of customization through a REST API](./customizing-cluster.html#getting-cluster-status) as described in the sections below.

The following example shows creating a cluster with customization in JSON format:

```
{
	"num_compute_nodes": 1,
	"hardware_config": "Standard",
	"software_package": "ae-0.1-SparkPack",
	"customization": [{
		"name": "action1",
		"type": "bootstrap",
		"script": {
			"source_type": "http",
			"script_path": http://10.141.81.55:8080/customaction.sh"
			},
		"script_params": []
        }]
}
```
{:codeblock}

`name` is the name of your customization action.
`type` is either `bootstrap` or `teardown`. Currently only `bootstrap`is supported.

The following example shows creating a cluster with customization by using the Cloud Foundry API:

```
Create Cluster with customization using CF API

  curl --request POST \
  --url 'https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true' \
  --header 'accept: application/json' \
  --header 'authorization: <User's UAA bearer token>' \
  --header 'cache-control: no-cache' \
  --header 'content-type: application/json' \
  --data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"fd6700e7-f8ba-4000-bb24-b1a63ce45d3a", "parameters": { "hardware_config":"Standard", "num_compute_nodes":1, "software_package":"ae-0.1-SparkPack", "customization":[<customization-details>]}}'

```
{:codeblock}

## Customization options

You can customize the following options:

- Operating system packages. You can install or remove operating system packages by using the package-admin tool.

The IBM Analytics Engine cluster comes bundled with a utility named `package-admin` that you can use to install or remove yum packages.

```
 sudo package-admin -c [install | remove] -p [package name]
```
{:codeblock}

- Python and R libraries. You can install custom Python and R libraries. The following two versions of Anaconda are installed on all nodes:

	* Anaconda with Python 2.7 at `/home/common/conda/anaconda2`
	* Anaconda with Python 3.5 at `/home/common/conda/anaconda3`

	In your script,  use commands like:

```
/home/common/conda/anaconda[2|3]/bin/pip install [python or R packages]
```   
{:codeblock}


- Ambari configurations. You can change Ambari configuration settings. Ambari configurations are only applicable on the management node. To ensure that these commands run only on the management node, add the following check to your script:

`if [ "x$NODE_TYPE" == "xmanagement" ]`

For example:

```
if [ "x$NODE_TYPE" == "xmanagement" ]
then
    echo "Updating ambari config properties"
    #change mapreduce.map.memory to 8192mb
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME mapred-site "mapreduce.map.memory.mb" "8192"
    # stop MAPREDUCE2 service
    curl -k -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Stop MAPREDUCE2"}, "ServiceInfo": {"state": "INSTALLED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services/MAPREDUCE2
    sleep 60
    # start MAPREDUCE2 service
    curl -k -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Start MAPREDUCE2"}, "ServiceInfo": {"state": "STARTED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services/MAPREDUCE2
fi
```
{:codeblock}

- Use of Swift or COS/S3 Object Storage. You can configure either Swift or COS/S3 Object Storage as a data source for Hadoop/Spark. For details see [Customizing clusters to use Swift and COS object stores](./Customizing-Cluster-to-use-Swift-and-COS-object-stores-as-data-source.html).

## Location of the customization script
  
You can add a customization script to the following sources:    

* HTTP (with or without basic authentication)

	For example:
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
{:codeblock}
	
* HTTPS

	For example:
```      
"customization":[{
    "name": "action1",
    "type": "bootstrap",
    "script": {
        "source_type": "https",
        "source_props": {
             "uri": "https://host:port/bootstrap.sh",
             "username": "user",
             "password": "pwd"
         },
         "script_path": "https://host:port/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
  }]
```
{:codeblock}

* Bluemix Object Storage

	For example:
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
{:codeblock}

For more detail on how to store your script and artifacts in Bluemix Object Storage and use the same script during customization see [samples page](./Customization-script-on-Bluemix-Object-Store.html). 

* SoftLayer Swift 
	
	For example:
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
{:codeblock}


* Softlayer COS S3

	For example:
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
{:codeblock}

## Running cluster management REST APIs 
			
To run cluster management REST APIs, you need to pass your IAM access token. To obtain the token, follow these [steps](./Retrieve-IAM-access-token.html).

### Rerunning a persisted customization script

A persisted customization script is registered during cluster creation and can be rerun. Enter the following command to rerun a persisted customization script:

```
curl -X POST -v "https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/customization_requests" -d '{"target":"all"}'  -H "Authorization: Bearer <user's IAM access token>" -H "Content-Type: application/json"
```
{:codeblock}


`target` can be:
   

- `all `: reruns the customization on all nodes
   

- `management`: reruns the customization only on the master management node
   

- `data`: reruns the customization on all data nodes  

- a comma separated list of fully qualified node names: reruns on the given list of nodes only.

### Running an adhoc customization script

An adhoc customization script can be run after the cluster was created and can only be run once. Enter the following command to run an adhoc customization script:

```
curl -X POST -v "https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/customization_requests" -d 
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
{:codeblock}


### Getting cluster status

Enter the following cluster management REST API to get cluster status information:

```
curl -i -X GET https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/state -H 'Authorization: Bearer <user's IAM access token>'

```
{:codeblock}

Expected Response: The cluster state is returned in JSON format, for example, ` {"state":"Active"}`

### Getting all customization requests for the given instance ID

Enter the following cluster management REST API to get the customization requests for the given instance ID:

```
curl -X GET https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/customization_requests -H 'Authorization: Bearer <user's IAM access token>'

```
{:codeblock}

Expected Response: The customization requests for the given service instance ID are returned in JSON format. For example:

```
[{"id":"37"},{"id":"38"}]
```

### Getting the details of a specific customization request

Enter the following cluster management REST API to get the details of a specific customization request:

```
curl -X GET https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/customization_requests/<request_id> -H 'Authorization: Bearer <user's IAM access token>'
			
```
{:codeblock}

Expected Response: The customization request details are returned in JSON format. For example:

```
{
	"id": "37",
	"run_status": "Completed",
	"run_details": {
		"overallStatus": "success",
		"details": [{
			"nodeName": "chs-fpw-933-mn001.bi.services.us-south.bluemix.net",
			"nodeType": "master-management",
			"startTime": "2017-06-06 11:46:35.519000",
			"endTime": "2017-06-06 11:47:46.687000",
			"timeTaken": "71 secs",
			"status": "CustomizeSuccess",
			"logFile": "/var/log/chs-fpw-933-mn001.bi.services.us-south.bluemix.net_37.log"
		}, {
			"nodeName": "chs-fpw-933-mn002.bi.services.us-south.bluemix.net",
			"nodeType": "management-slave1",
			"startTime": "2017-06-06 11:46:36.190000",
			"endTime": "2017-06-06 11:47:46.864000",
			"timeTaken": "70 secs",
			"status": "CustomizeSuccess",
			"logFile": "/var/log/chs-fpw-933-mn002.bi.services.us-south.bluemix.net_37.log"
		}, {
			"nodeName": "chs-fpw-933-dn001.bi.services.us-south.bluemix.net",
			"nodeType": "data",
			"startTime": "2017-06-06 11:46:36.693000",
			"endTime": "2017-06-06 11:47:47.271000",
			"timeTaken": "70 secs",
			"status": "CustomizeSuccess",
			"logFile": "/var/log/chs-fpw-933-dn001.bi.services.us-south.bluemix.net_37.log"
		}]
	}
}
```

