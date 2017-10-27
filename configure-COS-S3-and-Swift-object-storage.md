---

copyright:
  years: 2017
lastupdated: "2017-09-26"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Configuring clusters to work with IBM COS S3 object stores  

An application, such as Spark job or a Yarn job, can read data from or write data to an object store. Alternatively, the application itself, such as a Python file or a Yarn job jar, can reside in the object store. 

This section explains how to configure an IBM Analytics Engine cluster to connect to an object store to access data and applications stored in one of the following IBM object stores.

- Cloud Object Storage (COS S3) hosted on {{site.data.keyword.Bluemix_notm}}. This supports IBM IAM authentication which can be done by using the IAM API Key or the IAM token.

- Cloud Object Storage (COS S3) hosted on SoftLayer. This supports Amazon Web Services (AWS) style authentication.

**Restriction**: Starting with this release, Spark is the only component that can be used with IAM-based COS and Spark connects to the object store by using IBM Stocator. For more information about Stocator, see this [documentation](https://developer.ibm.com/open/openprojects/stocator/).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with object store credentials and other values. For achieving this, object store data like credentials, url etc. must be added to the core-site.xml as a set of key/value pairs. You can configure the object store by using one of the following three options:

* [Configure via the Ambari UI _after_ the cluster was created](#Configure-via-the-Ambari-UI-after-the-cluster-was-created).
* [Customize the cluster using a customization script](#Customize-the-cluster-using-a-customization-script).
* [Specify the properties at runtime](#Specify-the-properties-at-runtime).

Note that if you use the IAM token for IAM authentication to the object store, it is advisable to configure the core-site.xml file. If you are using the IAM token, it makes more sense to specify the properties at runtime as the token is temporary. Refer to examples for the IAM token based authentication parameters.


### Configure via the Ambari UI _after_ the cluster was created

#### Add properties and values to the core-site.xml file

To add the properties and values to your core-site.xml file on your cluster instance:

1. Open the Ambari console, and then the advanced configuration for HDFS.<br>
``` Ambari dashboard > HDFS > Configs > Advanced > Custom core-site > Add Property```
2. Add the properties and values.
3. Save your changes and restart any affected services. The cluster will have access to  your object store.

### Customize the cluster using a customization script
A customization script can be used when the cluster is created. This script includes the properties that need to be configured in the core-site.xml file and use the Ambari configs.sh to make the required changes.

#### Sample cluster customization script to configure the cluster with an AWS style authenticated object store

The following is a sample script for S3 COS object store. You can modify the script for various object stores that can be used based on the properties given in the following sections.
```
S3_ACCESS_KEY=<AccessKey-changeme>
S3_ENDPOINT=<EndPoint-changeme>
S3_SECRET_KEY=<SecretKey-changeme>

if [ "x$NODE_TYPE" == "xmaster-management" ]
then
    echo $AMBARI_USER:$AMBARI_PASSWORD:$AMBARI_HOST:$AMBARI_PORT:$CLUSTER_NAME

    echo "Node type is xmanagement hence updating ambari properties"
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.s3a.access.key" $S3_ACCESS_KEY
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.s3a.endpoint" $S3_ENDPOINT
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME core-site "fs.s3a.secret.key" $S3_SECRET_KEY

    echo "stop and Start Services"
    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Stop All Services via REST"}, "ServiceInfo": {"state":"INSTALLED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 200

    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Start All Services via REST"}, "ServiceInfo": {"state":"STARTED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 700
fi    
```
{: codeblock}

### Specify the properties at runtime
Alternatively, the properties can be specified at runtime in the Python, Scala or R code when executing jobs.

## Properties needed for various object stores
Each Object Storage has a different set of properties to be configured in the core-site.xml file.

### AWS style authentication parameters for IBM COS S3
Refer to https://ibm-public-cos.github.io/crs-docs/endpoints to help you decide on the endpoints you need to use based on your COS bucket type, such as regional vs cross-regional. Choose the PRIVATE endpoint listed. Using the public endpoint will be slower and more expensive.

An example of an EndPoint URL is `s3-api.us-geo.objectstorage.service.networklayer.com`.
```
fs.s3d.service.access.key=<Access Key ID>
fs.s3d.service.endpoint=<EndPoint URL>
fs.s3d.service.secret.key=<Secret Access Key>
```
{: codeblock}

### IBM IAM authentication parameters for IBM COS/S3 
Refer to https://ibm-public-cos.github.io/crs-docs/endpoints to help you decide on the endpoints you need to use based on your COS bucket type, such as regional vs cross-regional. Choose the PRIVATE endpoint listed. Using the public endpoint will be slower and more expensive.

An example of an EndPoint URL is `s3-api.us-geo.objectstorage.service.networklayer.com`.

Sample parameters are listed in the following section. Note that the value for <servicename> can be any literal such as `iamservice` or `myprodservice`. 

 - `fs.cos.<servicename>.v2.signer.type=false`. This must be set to false.

 - `fs.cos.<servicename>.endpoint=<EndPoint>`. For example, `s3-api.us-geo.objectstorage.service.networklayer.com`. This is the object store service’s endpoint.

 - `fs.cos.<servicename>.iam.service.id=<ServiceId>`. For example, `ServiceId-6f06c935-ffffff-3333dddd`. This is the IAM object store service’s ID.
 
 - `fs.cos.<servicename>.iam.endpoint=https://iam.bluemix.net/identity/token–`. This is the IAM server’s end point. This value is always fixed as shown here.

 - `fs.cos.<servicename>.iam.api.key=<IAM API Key>`. This is the IAM object store service’s API Key defined in the credentials of the service.

 - `fs.cos.<servicename>.iam.token=<IAM Token e.g -2342342sdfasf34234234asf……..>`. This will be the IAM token of an individual user that is obtained from the BX CLI oauth-tokens command. 

NOTE : You need to specify either the API key or the token. Keep in mind that the token  expires which means that it better to specify it at runtime rather than to define it in the core-site.xml file.

## Preconfigured properties
The core site configuration is pre-configured with the following properties. The relevant properties are provided below.
```
"fs.stocator.scheme.list":"cos" 
"fs.cos.impl":"com.ibm.stocator.fs.ObjectStoreFileSystem" 
"fs.stocator.cos.impl":"com.ibm.stocator.fs.cos.COSAPIClient" 
"fs.stocator.cos.scheme":"cos"
```
{:codeblock}

## URI patterns for objects in AWS authentication style object stores

`s3a://<bucket_name>/<object_name>` 

For example, `s3a://mybucket/detail.txt`

## URI pattern for objects in IBM IAM authenticated object stores

`cos://<bucket_name>.<servicename>/<object_name>`
 
For example, `cos://mybucket.myprodservice/detail.txt`

Note: Starting with this release, this will work only with Spark.
 
## Location of Stocator jars

If there is a need to test a patch, you can replace the jar at this location:
```
/home/common/lib/dataconnectorStocator
```
