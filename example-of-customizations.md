---

copyright:
  years: 2017,2018
lastupdated: "2018-05-15"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Examples of customizations

The following sections show you different examples of how you can customize a cluster.

For details on what to consider when customizing a cluster, see [Customizing a cluster](./customizing-cluster.html).

### Example of creating a cluster with bootstrap customization using the Cloud Foundry CLI

`cf create-service IBMAnalyticsEngine lite <service instance name> -c <cluster parameters as json string or path to cluster parameters json file>`

The following sample shows the parameters in JSON format:
```
"num_compute_nodes": 1,
	"hardware_config": "default",
	"software_package": "ae-1.0-spark",
	"customization": [{
		"name": "action1",
		"type": "bootstrap",
		"script": {
			"source_type": "https",
			"source_props": {},
			"script_path": "https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/associate-cos.sh"
		},
		"script_params": ["<s3_endpoint>", "<s3_access_key>", "<s3_secret_key>"]
	}]
```

Where:
- `name` is the name of your customization action. It can be any literal without special characters
- `type` is either `bootstrap` or `teardown`. Currently only `bootstrap` is supported.


**Note:** Currently, only one custom action can be specified in the `customization` array.

### Example of creating a cluster with bootstrap customization using the Cloud Foundry (cf) REST API

```
curl --request POST \
--url 'https://api.ng.bluemix.net/v2/service_instances?accepts_incomplete=true' \
--header 'accept: application/json' \
--header 'authorization: <User's UAA bearer token>' \
--header 'cache-control: no-cache' \
--header 'content-type: application/json' \
--data '{"name":"<Service instance name>", "space_guid":"<User's space guid>", "service_plan_guid":"acb06a56-fab1-4cb1-a178-c811bc676164", "parameters": { "hardware_config":"default", "num_compute_nodes":1, "software_package":"ae-1.0-spark", "customization":[<customization-details>]}}'
```

**Note:**
* If you need to find your UAA bearer token, see [Retrieving the Cloud Foundry UAA access token](./retrieving-uaa-access-token.html).
* If you need to find your space GUIDs, see [Obtaining the space GUID](./provisioning_deprecated.html#obtaining-the-space-guid).
* To run cluster management REST APIs, you need to pass your IAM access token. To obtain the token, follow these [steps](./Retrieve-IAM-access-token.html).
* For the United Kingdom region use the end point  `https://api.eu-gb.bluemix.net`.

### Example of running an adhoc customization script

An adhoc customization script can be run any time after the cluster is created and becomes active. Enter the following command to run an adhoc customization script for target `all`:

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
`name` is the name of your customization action. It can be any literal without special characters.

**Note:** For the United Kingdom region, use the end point `https://api.eu-gb.dataplatform.ibm.com`.

### Example of customizing Ambari configurations

The following section shows you a snippet of a customization script that you can use to customize Ambari configurations. This is also an example of how to use the predefined environment variable `NODE_TYPE`.

The example makes use of Ambari's in-built `configs.sh` script to change the value for `mapreduce.map.memory`. This script is available only on the management nodes. If you specified `target` as `all` for adhoc customization or if `all` target is implied because of a bootstrap customization, you might want to specify the `NODE_TYPE` so that the code will be executed only once and from the management slave2 node.

```
if [ "x$NODE_TYPE" == "xmanagement-slave2" ]
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

### Example of installing Python/R packages

There are two versions of Anaconda installed on all nodes:
 - Anaconda with Python 2.7 at `/home/common/conda/anaconda2`
 - Anaconda with Python 3.5 at `/home/common/conda/anaconda3`

In your customization script, use commands like:
`/home/common/conda/anaconda[2|3]/bin/pip install [python or R packages]`

### Example of configuring COS/S3 Object Storage as a data source for Hadoop/Spark

For details see [Configuring clusters to work with IBM COS S3 object stores](./configure-COS-S3-object-storage.html).

### Examples of different kinds of locations of the customization script

The following examples show snippets of the `script` and `script_params` attributes for various locations of the customization's JSON input. The customization script can be hosted on a Github repository (source_type:https) or in a bucket on S3 storage (source_type:CosS3). It can also be a Swift object store. (Note however that the Swift location is deprecated)


**Note:** The maximum number of characters that can be used in the `"script"` attribute of the JSON input is limited to 4096 chars.

#### Example of the script hosted in an Github repository
```
"script": {
    "source_type": "https",
    "source_props": {},
    "script_path": "https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/associate-cos.sh"
},
"script_params": ["CHANGEME_ENDPOINT", "CHANGE_ACCESS_KEY", "CHANGE_SECRET"]
```
`<CHANGEME_ENDPOINT>` is the endpoint of the IBM S3 instance, for example, `s3-api.sjc-us-geo.objectstorage.softlayer.net`.
`<CHANGE_ACCESS_KEY>` is the access key of the IBM S3 instance.
`<CHANGE_SECRET>` is the secret of the IBM S3 instance.

**NOTE:** The script path should be the raw content path of your script. The example uses a script that associates an S3 COS instance with the cluster so that data in S3 COS can be used in Hadoop and Spark jobs.

####  Example of the script hosted in an HTTPS location (with or without basic authentication)
```
    "script": {
        "source_type": "https",
        "source_props": {
             "username": "user",
             "password": "pwd"
         },
         "script_path": "https://host:port/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
```
#### Example of the customization script hosted in Softlayer COS S3
```
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
```

#### Example of the script hosted in a Bluemix Swift object store (deprecated)
```
    "script": {
        "source_type": "BluemixSwift",
        "source_props": {
           "auth_url": "https://identity.open.softlayer.com",
            "user_id": "xxxxxxxx",
           "password": "yyyyyyyyyy",
           "project_id": "zzzzzzzzz",
           "region": "dallas"
         },
         "script_path": "/myContainer/myFolder/bootstrap.sh"
    },
    "script_params": ["arg1", "arg2"]
```

#### Example of the script hosted in a SoftLayer Swift object store (deprecated)
```
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
```

### Example of re-running a bootstrap customization script registered during cluster creation

A persisted customization script is registered during cluster creation and can be rerun. Enter the following command to rerun a persisted customization script:
```
curl -X POST -v "https://api.dataplatform.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests" -d '{"target":"all"}'  -H "Authorization: Bearer <user's IAM access token>" -H "Content-Type: application/json"
```
**Note:** For the United Kingdom region, use the end point `https://api.eu-gb.dataplatform.ibm.com`.
