---

copyright:
  years: 2017
lastupdated: "2017-08-01"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Integrating IBM COS S3 and IBM Swift object stores

An application, such as Spark job or a Yarn job, can read data from or write data to an object store. Alternatively the application itself, such as a python file or a yarn job jar, can reside on the object store. This section will explain how to configure an IBM Analytics Engine cluster to connect to an object store to access data and applications stored in one of the following IBM object stores.

- Cloud Object Storage(COS S3) hosted on Bluemix/SL using Open Stack connectors
- Cloud Object Storage(COS S3) hosted on Bluemix/SL using IBM Stocator connectors
- Object Storage Regional(Swift) hosted on Bluemix(uses IBM Stocator connector internally)
- Object Storage Regional(Swift) hosted on SoftLayer(uses IBM Stocator connector internally)

**Restriction** Integration for object store using the Stocator connector is supported only for the Spark component for this release. While some of the other Hadoop components might work partially, there are some restrictions from using it fully and will not be supported for this release. To understand more about Stocator, go [here](https://developer.ibm.com/open/openprojects/stocator/).

## Configuration options

In order for an application to connect to an object store, the cluster configuration must be updated with object store credentials and other values. For achieving this, object store data like credentials, url etc. must be added to the core-site.xml as a set of key/value pairs. You can configure the object store by using one of the following three options:

### Configure via the Ambari UI _after_ the cluster was created

#### Add properies and values to the core-site.xml file

**To add the properies and values to your core-site.xml file on your cluster instance**

1. Open the Ambari console, and then the advanced configuration for HDFS.<br>
``` Ambari dashboard > HDFS > Configs > Advanced > Custom core-site > Add Property```
2. Add the properties and values.
3. Save your changes and restart any affected services. The cluster will have access to your object store.

#### Customize the cluster using a customization script
A customization script can be used when the cluster is created. This script includes the properties that need to be configured in the core-site.xml file and use the Ambari configs.sh to make the required changes.

#### Sample script for a cluster customization script

The following is a sample script for S3 COS object store. You can modify the script for various object stores that can be used based on the properties given in the following sections.
```
S3_ACCESS_KEY=<AccessKey-changeme>
S3_ENDPOINT=<EndPoint-changeme>
S3_SECRET_KEY=<SecretKey-changeme>

if [ "x$NODE_TYPE" == "xmanagement" ]
then
    echo $AMBARI_USER:$AMBARI_PASSWORD:$AMBARI_HOST:$AMBARI_PORT:$CLUSTER_NAME

    echo "Node type is xmanagement hence updating ambari properties"
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p
    $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME  core-site
    "fs.s3a.access.key" $S3_ACCESS_KEY
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p
    $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME  core-site
    "fs.s3a.endpoint" $S3_ENDPOINT
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p
    $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME  core-site
    "fs.s3a.secret.key" $S3_SECRET_KEY

    echo "stop and Start Services"
    curl -k -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X
    PUT -d '{"RequestInfo": {"context": "Stop All Services via REST"}, "ServiceInfo":
    {"state":"INSTALLED"}}'
    https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 100

    curl -k -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X
    PUT -d '{"RequestInfo": {"context": "Start All Services via REST"}, "ServiceInfo":
    {"state":"STARTED"}}'
    https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 700
fi
```
{: codeblock}

#### Specify the properties at runtime
Alternatively, the properties can be specified at runtime in the python/scala/R code when executing jobs. Refer to examples section below.

## Properties needed for various object stores
Each Object Storage has a different set of properties to be configured in the core-site.xml file.

### IBM COS/S3 OpenStack connector
Refer to https://ibm-public-cos.github.io/crs-docs/endpoints to decide on the endpoints you want to use.
```
fs.s3a.access.key=<Access Key ID>
fs.s3a.endpoint=<EndPoint URL>
fs.s3a.secret.key=<Secret Access Key>
```
{: codeblock}

### IBM COS/S3 with IBM Stocator connector
```
fs.s3d.service.access.key=<Access Key ID>
fs.s3d.service.endpoint=<EndPoint URL>
fs.s3d.service.secret.key=<Secret Access Key>
```
{: codeblock}

### IBM Bluemix Swift
Note that the swift uses stocator connector internally. "bmv3" referred to below is the service name of Bluemix Swift instance. You can use any identifier instead of "bmv3," as per your convenience, as long as you refer to it in the URI when referencing the object.
```
fs.swift.service.bmv3.auth.url=https://identity.open.softlayer.com/v3/auth/tokens
fs.swift.service.bmv3.public=true
fs.swift.service.bmv3.username=<userId on Bluemix>
fs.swift.service.bmv3.password=<password on Bluemix>
fs.swift.service.bmv3.tenant=<projectId on Bluemix>
fs.swift.service.bmv3.region=dallas
fs.swift.service.bmv3.auth.method=keystoneV3
```
{: codeblock}

### IBM SL Swift
"dal05" referred to below is the regionId of SoftLayer Swift instance. You can use any identifier, as per your convenience, instead of "dal05" as long as you refer to it in the uri when referencing the object.
```
# Example of an tenant/account name is "IBMOS288698-27745"
# Example of an username is "jsmith"
fs.swift.service.dal05.auth.method=swiftauth
fs.swift.service.dal05.auth.url=<authentication_end_point>
fs.swift.service.dal05.password=<API_Key_Password>
fs.swift.service.dal05.public=true
fs.swift.service.dal05.tenant=<account_name>
fs.swift.service.dal05.username=<username>
```
{:codeblock}

## Pre-configured properties
The core site configuration is pre-configured with the following properties. The relevant properties are provided below.
```
"fs.s3a.impl":"org.apache.hadoop.fs.s3a.S3AFileSystem"
"fs.s3d.impl":"com.ibm.stocator.fs.ObjectStoreFileSystem"
"fs.swift.blocksize":"102400"
"fs.swift.connect.timeout":"300000"
"fs.swift.partsize":"204800"
"fs.swift.socket.timeout":"300000"
```
{:codeblock}

The spark configuration is pre-configured with the following properties. Some relevant properties are given below. Note that this implies that using the swift:// uri to access objects uses the Stocator connector internally.
```
"spark.hadoop.fs.swift.impl":"com.ibm.stocator.fs.ObjectStoreFileSystem"
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
### Swift BMX objects
  ```
   - swift://<container_name>.<servicename>/<object_name>
   - e.g:- swift://mycontainer.bmv3/detail.txt
 ```
### Swift SL objects
 ```
  - swift://<container_name>.< servicename>/<object_name>
  - e.g:- swift://mycontainer.dal05/detail.txt
 ```
## Examples of Spark Submit with various object stores

The exact version of the jars used in the below examples will change depending on the release that you  are working on

### Swift SL
_Example of working with a spark job jar in swift object store in Softlayer_
```
cd <sparkhome>
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode cluster swift://mycontainer.dal05/spark/spark-examples.jar
```

_Example of working with a local example spark job with data in the swift object store in Softlayer_
```
cd <sparkhome>
./bin/spark-submit --jars /usr/iop/current/hbase-client/lib/htrace-core*-incubating.jar --class org.apache.spark.examples.mllib.JavaALS --master yarn-cluster ./examples/jars/spark-examples*.jar  swift://mycontainer.dal05/spark/test.data 1 1 swift://mycontainer.dal05/spark/javaALS-result
```

### Swift Bluemix
_Example of working with a spark job jar in swift object store in Bluemix_
```
cd <sparkhome>
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode cluster swift://mycontainer.bmv3/spark/spark-examples.jar
```

_Example of working with a local example spark job with data in the swift object store in Bluemix_
```
cd <sparkhome>
./bin/spark-submit --jars /usr/iop/current/hbase-client/lib/htrace-core*-incubating.jar --class org.apache.spark.examples.mllib.JavaALS --master yarn-cluster ./examples/jars/spark-examples*.jar  swift://mycontainer.bmv3/spark/test.data 1 1 swift://mycontainer.bmv3/spark/javaALS-result
```

### S3 COS
_Example of working with a spark job jar in S3 COS_
```
cd <sparkhome>
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode cluster s3a://mybucket/spark/spark-examples.jar
```

_Example of working with a local example spark job with data in the S3 COS_
```
cd <sparkhome>
./bin/spark-submit --jars /usr/iop/current/hbase-client/lib/htrace-core*-incubating.jar --class org.apache.spark.examples.mllib.JavaALS --master yarn-cluster ./examples/jars/spark-examples*.jar  s3a://mybucket/spark/test.data 1 1 s3a://mybucket/spark/javaALS-result
```

### S3 COS with Stocator connector
_Example of working with a spark job jar in S3 COS_
```
cd <sparkhome>
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode cluster s3d://mybucket.softlayer/spark/spark-examples.jar
```

_Example of working with a local example spark job with data in the S3 COS_
```
cd <sparkhome>
./bin/spark-submit --jars /usr/iop/current/hbase-client/lib/htrace-core*-incubating.jar --class org.apache.spark.examples.mllib.JavaALS --master yarn-cluster ./examples/jars/spark-examples*.jar  s3d://mybucket.softlayer/spark/test.data 1 1 s3d://mybucket.softlayer/spark/javaALS-result
```

## Examples of Livy execute with various object stores

Refer to [Livy-API](https://github.ibm.com/wdp-chs/knowledgebase/wiki/Livy-API) for more information on Livy

The file access uri patterns are similar to the spark-submit example above. Couple of examples are given below for reference.

### S3 COS example

_Example of running a Livy with the executor python file in S3 COS_
```
curl \
-u "clsadmin:mypassword" \
-H 'Content-Type: application/json' \
-d '{ "file":"s3a://mybucket/spark/pi.py" }' \
"https://chs-aaa-999-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
```

### Swift BMX example

_Example of running a Livy with the executor jar file in BMX Swift_
```
curl \
-u "clsadmin:mypassword" \
-H 'Content-Type: application/json' \
-d '{ "file":"swift://mycontainer.bmv3/spark-examples.jar", "className":"org.apache.spark.examples.SparkPi" }' \
"https://chs-aaa-999-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
```
## Examples of sample Python code to configure properties at runtime

For more elaboration refer to [DSX blog](http://datascience.ibm.com/blog/working-with-object-storage-in-data-science-experience-python-edition/).
```
 def set_hadoop_config_with_credentials(creds):
"""This function sets the Hadoop configuration so it is possible to
    access data from Bluemix Object Storage using Spark"""

    # you can choose any name
    name = 'keystone'

    prefix = 'fs.swift.service.' + name
    hconf = sc._jsc.hadoopConfiguration()
    hconf.set(prefix + '.auth.url', 'https://identity.open.softlayer.com'+'/v3/auth/tokens')
    hconf.set(prefix + '.auth.endpoint.prefix', 'endpoints')
    hconf.set(prefix + '.tenant', creds['project_id'])
    hconf.set(prefix + '.username', creds['user_id'])
    hconf.set(prefix + '.password', creds['password'])
    hconf.setInt(prefix + '.http.port', 8080)
    hconf.set(prefix + '.region', 'dallas')
    hconf.setBoolean(prefix + '.public', False)
```

## Location of Stocator jars

If there is a need to test a patch, you can replace the jar in this location
```
/home/common/lib/dataconnectorStocator
```
