---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Configuring clusters to work with IBM COS S3 object stores  

An application, such as Spark job or a Yarn job, can read data from or write data to an object store. Alternatively, the application itself, such as a Python file or a Yarn job jar, can reside in the object store.

This section explains how to configure an IBM Analytics Engine cluster to connect to an object store to access data and applications stored in one of the following IBM object stores. The following options are displayed when you search for “Object Storage”  in the {{site.data.keyword.Bluemix_notm}} catalog:

 - **Cloud Object Storage (COS S3)**. This supports IBM IAM authentication which can be done by using the IAM API Key or the IAM token. This object store can be accessed only by IBM connectors using the `cos://` scheme.

	Restriction: In the current release, this will work only for Spark.

 - **Cloud Object Storage (COS S3) IAA** . This supports Amazon Web Services (AWS) style authentication. This object store can be accessed using:

   - HDP open source AWS connectors by using the `s3a://` scheme.
   - Or IBM connectors by using the `cos://`scheme.

The Spark service uses Stocator libraries and offers better performance for large object reads and writes as compared to the open source connectors. To learn more about Stocator, see [here](https://github.com/SparkTC/stocator).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with object store credentials and other values. For this, object store data like the credentials, URL, etc. must be added to the core-site.xml file as a set of key/value pairs. You can configure the object store by using one of the following three options:

* [Configure via the Ambari UI _after_ the cluster was created](#Configure-via-the-Ambari-UI-after-the-cluster-was-created).
* [Customize the cluster using a customization script](#Customize-the-cluster-using-a-customization-script).
* [Specify the properties at runtime](#Specify-the-properties-at-runtime).

After you configured the cluster, you can access objects in the object store using HDFS commands, and run MR, Hive and Spark jobs on them.

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

Alternatively, the properties can be specified at runtime in the Python, Scala or R code when executing jobs. The following snippet shows an example for Spark:

```
prefix="fs.cos.myfirstservice"

hconf=sc._jsc.hadoopConfiguration()
hconf.set(prefix + ".iam.endpoint", "https://iam.bluemix.net/identity/token")
hconf.set(prefix + ".endpoint", "s3-api.us-geo.objectstorage.service.networklayer.com")
hconf.set(prefix + ".iam.api.key", "he0Zzjasdfasdfasdfasdfasdfasdfj2OV")
hconf.set(prefix + ".iam.service.id", "ServiceId-asdf-asdf-asdf-asdf-asdf")

t1=sc.textFile("cos://mybucket.myfirstservice/tata.data")
t1.count()
```     
{: codeblock}


## Properties needed for various object stores

Each Object Storage has a different set of properties to be configured in the core-site.xml file.

NOTE: Refer to [Selecting endpoints](https://ibm-public-cos.github.io/crs-docs/endpoints) to help you decide on the endpoints you need to use based on your COS bucket type, such as regional versus cross-regional. In the case of an IAM authenticated object store, you can refer to the EndPoints tab of the service instance page. Choose the PRIVATE endpoint listed. Using the public endpoint is  slower and more expensive.

An example of an endpoint URL is `s3-api.us-geo.objectstorage.service.networklayer.com`.

### AWS style authentication parameters using the HDP connector

```  
fs.s3a.access.key=<Access Key ID>
fs.s3a.endpoint=<EndPoint URL>
fs.s3a.secret.key=<Secret Access Key>
```

### AWS style authentication parameters using the IBM connector
Note that the value for `<servicename>` can be any literal such as `awsservice` or `myobjectstore`.

```
fs.cos.<servicename>.access.key=<Access Key ID>
fs.cos.<servicename>.endpoint=<EndPoint URL>
fs.cos.<servicename>.secret.key=<Secret Access Key>
```

### IAM style authentication parameters using the IBM connector
Note that the value for `<servicename>` can be any literal such as `iamservice` or `myprodservice`.  You can use `<servicename>` and define multiple sets of parameters to demarcate different instances of the object store.

```
fs.cos.<servicename>.v2.signer.type=false  # This must always be set to false.
fs.cos.<servicename>.endpoint=<EndPoint e.g:s3-api.us-geo.objectstorage.service.networklayer.com>. This is the object store service’s endpoint.
fs.cos.<servicename>.iam.service.id=<ServiceId e.g:ServiceId-6f06c935-ffffff-3333dddd> This is the IAM object store service’s ID.
fs.cos.<servicename>.iam.endpoint=https://iam.bluemix.net/identity/token #This is the IAM server’s end point. This value is always fixed as shown here.
fs.cos.<servicename>.iam.api.key=<IAM API Key> #This is the IAM object store service’s API Key defined in the credentials of the service.
fs.cos.<servicename>.iam.token=<IAM Token e.g:- 2342342sdfasf34234234asf……..> #This will be the IAM token of an individual user that is obtained from the BX CLI oauth-tokens command.
```

NOTE : You need to specify either the API key or the token. Keep in mind that the token  expires which means that it better to specify it at runtime rather than to define it in the core-site.xml file.

## Preconfigured properties
The core site configuration is preconfigured with properties. The following  properties are relevant.
```
"fs.stocator.scheme.list":"cos"
"fs.cos.impl":"com.ibm.stocator.fs.ObjectStoreFileSystem"
"fs.stocator.cos.impl":"com.ibm.stocator.fs.cos.COSAPIClient"
"fs.stocator.cos.scheme":"cos"
```
{:codeblock}

## URI pattern for accessing objects using HDP open source AWS connectors

`s3a://<bucket_name>/<object_name>`
For example, `s3a://mybucket/detail.txt`

## URI pattern for accessing objects using IBM COS connectors

`cos://<bucket_name>.<servicename>/<object_name>`
For example, `cos://mybucket.myprodservice/detail.txt`
