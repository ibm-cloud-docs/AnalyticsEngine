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

# Running Spark apps stored in Object Storage

## Description
This section will explain the configuration required to submit a Spark application that is persisted in IBM Bluemix Object Storage to an IBM Analytics Engine cluster. A Spark application can always read data from or write data to an Object Storage.  Here we are focused on running a Spark application where the application itself is persisted in Object Storage.

Note: access to any persistent store is dependent on the data connectors available in the cluster.
See [Connectors supported in IBM Analytics Engine](./supported-connectors.html).

The configuration and example commands below are based on using an IBM Bluemix Object Storage with access via the swift2d/stocator protocol (`swift2d://` URL).   Other Object Storage services would be handled in a similar fashion, with minor changes to URL's or credential configuration information.


## Prerequisites
It is assumed that you have already provisioned a cluster, and you understand how to open the Ambari console for cluster configuration, and to submit Spark jobs. If not, refer to the IBM Analytics Engine [Getting Started](./staging/index.html) documentation.

It is also assumed that you have already provisioned an Object Storage instance, know how to access your Object Storage, and how to add or remove files.  In addition you need to have the Spark application that you desire to run stored inside a container in your Object Storage.

## High Level Steps
  1. Modify the cluster configuration to add Object Storage credentials.
  2. Invoke Livy to submit your Spark job using a reference to your application in Object Storage.

## Modify Cluster Configuration
The cluster configuration must be updated with Object Storage credential information to enable connecting to Object Storage and downloading the Spark application to the cluster.

The steps that will need to be completed are as follows:
  1. Retrieve information on how to access your Object Storage.
  2. Add the Object Storage credential information to the `core-site.xml` file on the cluster.


### Retrieve Object Store credential information
For access to the IBM Bluemix Object Store via the swift2d protocol you will need to configure the following six properties:
  1. `auth.url`   - from service credentials:`auth_url`
  2. `region`     - from service credentials:`region`
  3. `tenant`	- from service credentials:`projectId`
  4. `username`	- from service credentials:`userId`
  5. `password`	- from service credentials:`password`
  6. `public`	- boolean value (set to `false` )

The last property (`public`) is technically optional and is a statement on whether to use the "public" or "private" network for the communication.  Historically, setting this to "false" resulted in improved performance and was the recommended value.


### Add Object Storage credentials to the cluster
The information above is retrieved from the Bluemix Object Storage instance credentials  data, and must be added to the `core-site.xml` file as a set of properties.
For the swift2d protocol, each property definition must begin with the following prefix:
  * `fs.swift2d.service.<service_ref>`

Where the service reference (`<service_ref>`) is any name you choose to represent this Object Storage connection.  For example, if you choose the name `ObjStrA`, the properties would be:

```
fs.swift2d.service.ObjStrA.auth.url=https://identity.open.softlayer.com/v3/auth/tokens
fs.swift2d.service.ObjStrA.region=dallas
fs.swift2d.service.ObjStrA.tenant=<projectId>
fs.swift2d.service.ObjStrA.username=<userId>
fs.swift2d.service.ObjStrA.password=<password>
fs.swift2d.service.ObjStrA.public=false
```

To add the properties and values to your `core-site.xml` file on your cluster instance, open the Ambari console and then the advanced configuration for HDFS:
  * `Ambari dashboard --> HDFS --> Configs --> Advanced --> Cusom core-site --> Add Property...`

In this dialog add the six properties and values, save your changes, and restart any affected services.   After that the cluster will have access to your Object Storage using the `container name`, `service_ref` and `path/file` you specified. 

Example URLs:
  * `swift2d://<container>.<service_ref>/<path><file>`
  * `swift2d://MyApps.ObjStrA/PiEx.py`

If you need connections to multiple Object Storage instances you can add more connection properties and reference them accordingly.

## Submit Spark Job via Livy
Using Livy to submit a Spark application that exists in Object Storage is basically the same as submitting any Spark application.  The only difference is the "File" reference will be an Object Storage URL.

Example File References
  * local file system - `local:/usr/iop/current/spark2-client/jars/spark-examples.jar`
  * HDFS file system - `hdfs://<host>:<port>/user/clsadmin/PiEx.py`
  * swift object store - `swift2d://<container>.<service_ref>/PiEx.py`

Therefore when submitting a Spark application to Livy that resides in Object Storage you simply specify the file reference as an Object Storage URL.

### Examples
Generic command to submit a Spark application residing in Object Storage via Livy:
```
curl -k \
-u "<user>:<password>" \
-H 'Content-Type: application/json' \
-d '{ "file":"swift2d://<container>.<service_ref>/<path_file_name>" }' \
"https://<clusterhost>:8443/gateway/default/livy/v1/batches"
```

Given some example values, the command would be as follows:
  * IBM Analytics Engine cluster
      - host = *wce-tmp-867-mn001.bi.services.us-south.bluemix.net*
  * Object Storage
      - container name = *MyApps*
      - service reference = *ObjStrA*
      - spark application = *PiEx.py*

```
curl -k \
-u "<user>:<password>" \
-H 'Content-Type: application/json' \
-d '{ "file":"swift2d://MyApps.ObjStrA/PiEx.py" }' \
"https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
```

If the application was Java/Scala based and the jar file was stored in Object Storage, the command would need to specify both a reference to the jar file and the class you wanted to run like this:
```
curl -k \
-u "<user>:<password>" \
-H 'Content-Type: application/json' \
-d '{ "file":"swift2d://MyApps.ObjStrA/spark-examples_2.10-2.1.0.jar", "className":"org.apache.spark.examples.SparkPi" }' \
"https://iae-tmp-867-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
```
