---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-21"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Configuring clusters to work with IBM COS S3 object stores  

An application, such as a Spark job or a Yarn job, can read data from or write data to an object store. Alternatively, the application itself, such as a Python file or a Yarn job jar, can reside in the object store.

This section explains how to configure an {{site.data.keyword.iae_full_notm}} cluster to connect to an object store to access data and applications stored in one of the following styles of IBM object stores. When you search for “Object Storage”  in the {{site.data.keyword.Bluemix_notm}} catalog, you may get an option to choose from one of the following:

 - **Cloud Object Storage (COS S3)**. This supports IBM IAM authentication which can be done by using the IAM API Key or the IAM token. Using IAM tokens gives you a fine grained control over user access to buckets.   

 - **Cloud Object Storage (infrastructure)** . This supports Amazon Web Services (AWS) style authentication.


 To learn more about COS and its authentication mechanisms, click [here](https://console.bluemix.net/docs/services/cloud-object-storage/about-cos.html#about-ibm-cloud-object-storage).  

 Both of these styles of IBM S3 COS object store instances can be accessed using the `cos://` scheme. Details of the configuration parameters you need and the URI to access objects is described in the following sections. You can also access Amazon AWS object store instances from the cluster using the `s3a://` scheme.

 Spark jobs in particular use open source Stocator libraries and offer better performance for large object reads and writes as compared to the default AWS connectors. To learn more about Stocator, see [here](https://github.com/SparkTC/stocator).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with object store credentials and other values. For this, object store data like the credentials, URL, etc. must be added to the core-site.xml file as a set of key/value pairs. You can configure the object store by using one of the following three options:

* [Configure via the Ambari UI after the cluster was created](./configure-cos-via-ambari.html)
* [Customize the cluster using a customization script](#customize-the-cluster-using-a-customization-script)
* [Specify the properties at runtime](#specify-the-properties-at-runtime)

After you configured the cluster, you can access objects in the object store using HDFS commands, and run MR, Hive and Spark jobs on them.

### Customize the cluster using a customization script
You can use a customization script when the cluster is created. This script includes the properties that need to be configured in the core-site.xml file and use the Ambari configs.sh file to make the required changes.

#### Sample cluster customization script to configure the cluster with an AWS style authenticated object store

The following example shows a sample script for an AWS authentication style S3 COS object store. You can modify the script for IAM authentication style object stores that can be used based on the properties given in the following sections. The  sample restarts only the affected services and, instead of sleeping for a long random interval after firing the Ambari API,  the progress is polled via the Ambari API.
```
#!/bin/bash
#------------------------------------------------------------------------------
# Customization script to associate a COS instance with an IAE
# cluster. It expects COS credentials in AWS style. Specifically these three
# arguments: <s3_endpoint> <s3_access_key> <s3_secret_key>
#------------------------------------------------------------------------------

# Helper functions

# Parse json and return value for the specified json path
parseJson ()
{
	jsonString=$1
	jsonPath=$2

	echo $(echo $jsonString | python -c "import json,sys; print json.load(sys.stdin)$jsonPath")
}

# Track progress using the call back returned by Ambari restart API
trackProgress ()
{
	response=$1
	# Extract call back to from response to track progress
	progressUrl=$(parseJson "$response" '["href"]')
	echo "Link to track progress: $progressUrl"

	# Progress tracking loop
	tempPercent=0
    while [ "$tempPercent" != "100.0" ]
	do
        progressResp=`curl -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By:ambari' -X GET $progressUrl --silent`
		tempPercent=$(parseJson "$progressResp" '["Requests"]["progress_percent"]')
		echo "Progress: $tempPercent"
		sleep 5s
	done

	# Validate if restart has really succeeded
	if [ "$tempPercent" == "100.0" ]
	then
		# Validate that the request is completed
		progressResp=`curl -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By:ambari' -X GET $progressUrl --silent`
		finalStatus=$(parseJson "$progressResp" '["Requests"]["request_status"]')
		if [ "$finalStatus" == "COMPLETED" ]
        then
        	echo 'Restart of affected service succeeded.'
            exit 0
        else
        	echo 'Restart of affected service failed'
            exit 1
        fi
	else
		echo 'Restart of affected service failed'
		exit 1
	fi
}

# Validate input
if [ $# -ne 3 ]
then
	 echo "Usage: $0 <s3_endpoint> <s3_access_key> <s3_secret_key>"
else
	S3_ENDPOINT="$1"
	S3_ACCESS_KEY="$2"
	S3_SECRET_KEY="$3"
fi

# Actual customization starts here
if [ "x$NODE_TYPE" == "xmanagement-slave2" ]
then    
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.cos.myprodservice.access.key" $S3_ACCESS_KEY
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.cos.myprodservice.endpoint" $S3_ENDPOINT
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.cos.myprodservice.secret.key" $S3_SECRET_KEY

    echo 'Restart affected services'
    response=`curl -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By: ambari' --silent -w "%{http_code}" -X POST -d '{"RequestInfo":{"command":"RESTART","context":"Restart all required services","operation_level":"host_component"},"Requests/resource_filters":[{"hosts_predicate":"HostRoles/stale_configs=true"}]}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/requests`

    httpResp=${response:(-3)}
    if [[ "$httpResp" != "202" ]]
    then
		echo "Error initiating restart for the affected services, API response: $httpResp"
		exit 1
    else
		echo "Request accepted. Service restart in progress...${response::-3}"
		trackProgress "${response::-3}"
    fi
fi
```     
{: codeblock}

For the complete source code of the customization script to associate a COS instance with an  {{site.data.keyword.iae_full_notm}} cluster, see [here]( https://github.com/IBM-Cloud/IBM-Analytics-Engine/blob/master/customization-examples/associate-cos.sh).

### Specify the properties at runtime

Alternatively, you can specify the properties at runtime in the Python, Scala, or R code when executing jobs. The following snippet shows an example for Spark:

```
prefix="fs.cos.myprodservice"

hconf=sc._jsc.hadoopConfiguration()
hconf.set(prefix + ".iam.endpoint", "https://iam.bluemix.net/identity/token")
hconf.set(prefix + ".endpoint", "s3-api.us-geo.objectstorage.service.networklayer.com")
hconf.set(prefix + ".iam.api.key", "he0Zzjasdfasdfasdfasdfasdfasdfj2OV")
hconf.set(prefix + ".iam.service.id", "ServiceId-asdf-asdf-asdf-asdf-asdf")

t1=sc.textFile("cos://mybucket.myprodservice/tata.data")
t1.count()
```     
{: codeblock}


## Authentication properties for object stores

Each style of authentication to a COS S3 object store has a different set of properties that need to be configured in the core-site.xml file.

**NOTE:** Refer to [Selecting endpoints](https://ibm-public-cos.github.io/crs-docs/endpoints) for help on which endpoints you need to use based on your COS bucket type, such as regional versus cross-regional. In the case of an IAM authenticated object store, you can refer to the EndPoints tab of the service instance page. Choose the PRIVATE endpoint listed. Using the public endpoint is slower and more expensive.

An example of an endpoint URL is `s3-api.us-geo.objectstorage.service.networklayer.com`.

### AWS style authentication parameters
Note that the value for `<servicename>` can be any literal such as `awsservice` or `myobjectstore`.

```
fs.cos.<servicename>.access.key=<Access Key ID>
fs.cos.<servicename>.endpoint=<EndPoint URL>
fs.cos.<servicename>.secret.key=<Secret Access Key>
```

### IAM style authentication parameters
Note that the value for `<servicename>` can be any literal such as `iamservice` or `myprodservice`.  You can use `<servicename>` and define multiple sets of parameters to demarcate different instances of the object store.

```
fs.cos.<servicename>.v2.signer.type=false  # This must always be set to false.
fs.cos.<servicename>.endpoint=<EndPoint e.g:s3-api.us-geo.objectstorage.service.networklayer.com>. This is the object store service’s endpoint.
fs.cos.<servicename>.iam.api.key=<IAM API Key> #This is the IAM object store service’s API Key defined in the credentials of the service.
fs.cos.<servicename>.iam.token=<IAM Token e.g:- 2342342sdfasf34234234asf……..> #This will be the IAM token of an individual user that is obtained from the BX CLI oauth-tokens command.
```
**NOTE:** You need to specify either the API key or the token. Keep in mind that the token expires which means that it is better to specify it at runtime rather than to define it in the core-site.xml file.

## URI pattern for accessing objects using IBM COS connectors

`cos://<bucket_name>.<servicename>/<object_name>`

For example, `cos://mybucket.myprodservice/detail.txt`
