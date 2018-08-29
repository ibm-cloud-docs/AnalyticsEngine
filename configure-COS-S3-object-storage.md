---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-28"

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

By default, Cloud Object Storage uses IAM-style credentials. If you want to work with AWS-style credentials, you need to provide the inline configuration parameter `{"HMAC":true}` as shown [here](https://console.bluemix.net/docs/services/cloud-object-storage/iam/service-credentials.html#service-credentials).

Both of these styles of IBM S3 COS object store instances can be accessed using the `cos://` scheme. Details of the configuration parameters you need and the URI to access objects is described in the following sections. You can also access Amazon AWS object store instances from the cluster using the `s3a://` scheme.

Spark jobs in particular use open source Stocator libraries and offer better performance for large object reads and writes as compared to the default AWS connectors. To learn more about Stocator, see [here](https://github.com/SparkTC/stocator).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with the object store credentials and other values. For this, any object store data like the credentials or  any URLs, must be added to the core-site.xml file as a set of key/value pairs. You can configure the object store by using one of the following three options:

* [Create an {{site.data.keyword.iae_full_notm}} service instance using advanced custom provisioning options](./advanced-provisioning-options.html)
* [Specify the properties at runtime](#specify-the-properties-at-runtime)
* [Customize the cluster using a customization script](#customize-the-cluster-using-a-customization-script)
* [Configure the cluster via the Ambari UI after it was created](./configure-cos-via-ambari.html)


After you configured the cluster, you can access objects in the object store using HDFS commands, and run MR, Hive and Spark jobs on them.

### Customize the cluster using a customization script

You can use a customization script when the cluster is created. This script includes the properties that need to be configured in the core-site.xml file and uses the Ambari configs.py file to make the required changes.

#### Sample cluster customization script to configure the cluster with an AWS style authenticated object store

Use this [customization sample script](https://github.com/IBM-Cloud/IBM-Analytics-Engine/blob/master/customization-examples/associate-cos.sh) to configure the {{site.data.keyword.iae_full_notm}} cluster for an AWS authentication style S3 COS object store.

The example shows the complete source code of the customization script. You can modify the script for IAM authentication style object stores that can be used based on the properties given in the following sections. The sample restarts only the affected services and, instead of sleeping for a long random interval after firing the Ambari API, the progress is polled via the Ambari API.

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
