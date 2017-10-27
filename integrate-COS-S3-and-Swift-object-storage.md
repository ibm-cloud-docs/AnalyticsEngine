---

copyright:
  years: 2017
lastupdated: "2017-09-22"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Configuring clusters to work with IBM COS S3 object stores

An application, such as Spark job or a Yarn job, can read data from or write data to an object store. Alternatively, the application itself, such as a python file or a yarn job jar, can reside on the object store. This section will explain how to configure an IBM Analytics Engine cluster to connect to an object store to access data and applications stored in one of the following IBM object stores.

- Cloud Object Storage(COS S3) hosted on {{site.data.keyword.Bluemix_notm}}/SL using Open Stack connectors
- Cloud Object Storage(COS S3) hosted on {{site.data.keyword.Bluemix_notm}}/SL using IBM Stocator connectors

**Restriction**: Integration for object store using the Stocator connector is supported only for the Spark and MapReduce components for this release. While some of the other Hadoop components might work partially using the Stocator connector, there are some restrictions from using it fully and will not be supported for this release. To understand more about Stocator, go [here](https://developer.ibm.com/open/openprojects/stocator/).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with object store credentials and other values. For achieving this, object store data like credentials, url etc. must be added to the core-site.xml as a set of key/value pairs. You can configure the object store by using one of the following three options:

* [Configure via the Ambari UI _after_ the cluster was created](#Configure-via-the-Ambari-UI-after-the-cluster-was-created).
* [Customize the cluster using a customization script](#Customize-the-cluster-using-a-customization-script).
* [Specify the properties at runtime](#Specify-the-properties-at-runtime).


### Configure via the Ambari UI _after_ the cluster was created

#### Add properties and values to the core-site.xml file

**To add the properties and values to your core-site.xml file on your cluster instance**

1. Open the Ambari console, and then the advanced configuration for HDFS.<br>
``` Ambari dashboard > HDFS > Configs > Advanced > Custom core-site > Add Property```
2. Add the properties and values.
3. Save your changes and restart any affected services. The cluster will have access to your object store.

### Customize the cluster using a customization script
A customization script can be used when the cluster is created. This script includes the properties that need to be configured in the core-site.xml file and use the Ambari configs.sh to make the required changes.

#### Sample script for a cluster customization script

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
    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Stop All Services via REST"}, "ServiceInfo": {"state":"INSTALLED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services sleep 200

    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Start All Services via REST"}, "ServiceInfo": {"state":"STARTED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services sleep 700
fi    
```
{: codeblock}

### Specify the properties at runtime
Alternatively, the properties can be specified at runtime in the python/scala/R code when executing jobs.

## Properties needed for various object stores
Each Object Storage has a different set of properties to be configured in the core-site.xml file.

### IBM COS/S3 OpenStack connector
Refer to https://ibm-public-cos.github.io/crs-docs/endpoints to decide on the endpoints you want to use. An example of an EndPoint URL is: s3-api.us-geo.objectstorage.softlayer.net.
```
fs.s3a.access.key=<Access Key ID>
fs.s3a.endpoint=<EndPoint URL>
fs.s3a.secret.key=<Secret Access Key>
```
{: codeblock}

### IBM COS/S3 with IBM Stocator connector
An example of an EndPoint URL is: s3-api.us-geo.objectstorage.softlayer.net.
```
fs.s3d.service.access.key=<Access Key ID>
fs.s3d.service.endpoint=<EndPoint URL>
fs.s3d.service.secret.key=<Secret Access Key>
```
{: codeblock}

## Pre-configured properties
The core site configuration is pre-configured with the following properties. The relevant properties are provided below.
```
"fs.s3a.impl":"org.apache.hadoop.fs.s3a.S3AFileSystem"
"fs.s3d.impl":"com.ibm.stocator.fs.ObjectStoreFileSystem"
```
{:codeblock}

## URI patterns to access files on the object store

### S3a/COS objects
 ```
 - s3a://<bucket_name>/<object_name>
 - e.g:- s3a://mybucket/detail.txt
 ```
### S3a/COS objects using stocator connector
 ```
 - s3d://<bucket_name>.softlayer/<object_name>
 - e.g:- s3d://mybucket.softlayer/detail.txt
 ```
## Location of Stocator jars

If there is a need to test a patch, you can replace the jar in this location
```
/home/common/lib/dataconnectorStocator
```
