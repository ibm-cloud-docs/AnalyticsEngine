---

copyright:
  years: 2017,2018
lastupdated: "2018-11-12"

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

The recommended method to customize Ambari components is to create the {{site.data.keyword.iae_full_notm}} service instance using [advanced custom provisioning options](./advanced-provisioning-options.html).

### Example of creating a cluster with bootstrap customization using the {{site.data.keyword.Bluemix_short}} CLI

Create a cluster with bootstrap customization using the {{site.data.keyword.Bluemix_short}} CLI:

`ibmcloud resource service-instance-create <service instance name> ibmanalyticsengine <Plan name> <region> -p @<path to JSON file with cluster parameters>`

The following sample shows the parameters in JSON format:
```
"num_compute_nodes": 1,
	"hardware_config": "default",
	"software_package": "ae-1.1-spark",
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
- `name` is the name of your customization action. It can be any literal without special characters.
- `type` is either `bootstrap` or `teardown`. Currently only `bootstrap` is supported.


Currently, only one custom action can be specified in the `customization` array.

### Example of creating a cluster with bootstrap customization using the {{site.data.keyword.Bluemix_short}} Resource Controller (rc) REST API

Create a cluster with bootstrap customization using the {{site.data.keyword.Bluemix_short}} Resource Controller (rc) REST API:

```
curl \
  --request POST \
  --url 'https://resource-controller.bluemix.net/v1/resource_instances'   \
  --header 'accept: application/json'   \
  --header 'authorization: Bearer <IAM token>'   \
  --data  @provision.json

cat provision.json
{
    "name": "MyServiceInstance",
    "resource_plan_id": "7715aa8d-fb59-42e8-951e-5f1103d8285e ",
    "resource_group_id": "XXXXX",
    "region_id": "us-south",
    "parameters": {
        "hardware_config": "default",
        "num_compute_nodes": "1",
        "software_package": "ae-1.1-spark",
	  “customization”: [<customization details>]
    }    
}

```

Consider the following aspects:
* Possible values for `resource_plan_id` and instructions on how to get the resource group ID are specified [here](./provisioning.html#creating-a-service-instance-using-the-resource-controller-rest-api).
* For the United Kingdom region ID, use `eu-gb`. For Germany, use `eu-de` and for Tokyo, use `jp-tok`.
* To obtain an IAM token, follow these [steps](./Retrieve-IAM-access-token.html). You also need this token for authentication when using cluster management REST APIs.

### Example of running an adhoc customization script

An adhoc customization script can be run any time after the cluster is created and becomes active. Enter the following command to run an adhoc customization script for target `all`:

```
curl -X POST -v " https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests" -d
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

**Note:** For the United Kingdom region, use the end point `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use `https://api.eu-de.ae.cloud.ibm.com` and for Tokyo, use `https://api.jp-tok.ae.cloud.ibm.com`.

### Example of customizing Ambari configurations

The following section shows you a snippet of a customization script that you can use to customize Ambari configurations. This is also an example of how to use the predefined environment variable `NODE_TYPE`.

The recommended method to customize Ambari components is to create the {{site.data.keyword.iae_full_notm}} service instance using [advanced custom provisioning options](./advanced-provisioning-options.html).

The following example makes use of Ambari's in-built `configs.py` script to change the value for `mapreduce.map.memory`. This script is available only on the management nodes. If you specified `target` as `all`  for adhoc customization or if `all` target is implied because of a bootstrap customization, you might want to specify the `NODE_TYPE` so that the code will be executed only once and from the management slave2 node.

Note that the sample only works for new HDP 2.6.2 or 2.6.5 clusters. For existing HDP 2.6.2 clusters, you must use the `configs.sh` script for cluster customization.

```
if [ "x$NODE_TYPE" == "xmanagement-slave2" ]
then
    echo "Updating ambari config properties"
    #change mapreduce.map.memory to 8192mb

    python /var/lib/ambari-server/resources/scripts/configs.py -s https --user=$AMBARI_USER --password=$AMBARI_PASSWORD --port=$AMBARI_PORT --action=set --host=$AMBARI_HOST --cluster=$CLUSTER_NAME --config-type=mapred-site -k "mapreduce.map.memory.mb" -v "8192"

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

### Python 3

To install Python 3.5 libraries, your script must install to the `/home/common/conda/anaconda3` environment by using:

 ```
 pip install <package-name>
 ```

 If you install from a local or remote archive, use:

 ```
 pip install <archive url or or local file path>
 ```

### Python 2

To install Python 2.7 libraries, your script must install to the `/home/common/conda/anaconda2` environment by setting some environment variable:

 ```
 export PATH=/home/common/conda/anaconda2/bin:$PATH
 export PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python
 export PYTHONPATH=~/pipAnaconda2Packages/
 export PIP_CONFIG_FILE=/home/common/conda/anaconda2/etc/pip.conf
 ```
 
 ```
 pip install <package-name>
 ```

 If you install from a local or remote archive, use:

 ```
 pip install <archive url or local file path>
 ```

For more information, see [Installing additional libraries](./installing-additional-libraries.html#installing-additional-libraries). 

### Example of configuring COS/S3 Object Storage as a data source for Hadoop/Spark

For details on configuring COS/S3 Object Storage as a data source for Hadoop/Spark, see [Configuring clusters to work with IBM COS S3 object stores](./configure-COS-S3-object-storage.html).

The recommended method to customize Ambari components is to create the {{site.data.keyword.iae_full_notm}} service instance using [advanced custom provisioning options](./advanced-provisioning-options.html).

### Examples of different kinds of locations of the customization script

The following examples show snippets of the `script` and `script_params` attributes for various locations of the customization's JSON input. The customization script can be hosted on a Github repository (source_type:https) or in a bucket on S3 storage (source_type:CosS3). It can also be a Swift object store. (Note however that the Swift location is deprecated)

The maximum number of characters that can be used in the `"script"` attribute of the JSON input is limited to 4096 chars.

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
curl -X POST -v " https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_id>/customization_requests" -d '{"target":"all"}'  -H "Authorization: Bearer <user's IAM access token>" -H "Content-Type: application/json"
```
For the United Kingdom region, use the endpoint `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use `https://api.eu-de.ae.cloud.ibm.com`, and for Tokyo `https://api.jp-tok.ae.cloud.ibm.com`.
