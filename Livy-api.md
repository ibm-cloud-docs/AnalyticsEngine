---

copyright:
  years: 2017
lastupdated: "2017-07-17"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Livy API

On the Analytics Engine cluster, Livy batches REST API is exposed at:

```
https://<management-node>:8443/gateway/default/livy/v1/batches/
```

For interactive workloads see [Spark Interactive](./spark-interactive-notebooks-api.html).

## REST API

Method | Endpoint                                                                             | Description
------ | ------------------------------------------------------------------------------------ | ----------------------------
GET    | [/batches](https://github.com/cloudera/livy#get-batches)                             | List all batch jobs
POST   | [/batches](https://github.com/cloudera/livy#post-batches)                            | Submit a batch job
GET    | [/batches/{batchId}](https://github.com/cloudera/livy#get-batchesbatchid)            | Return batch job information
DELETE | [/batches/{batchId}](https://github.com/cloudera/livy#delete-batchesbatchid)         | Kills the batch job
GET    | [/batches/{batchId}/state](https://github.com/cloudera/livy#get-batchesbatchidstate) | Return batch job state
GET    | [/batches/{batchId}/log](https://github.com/cloudera/livy#get-batchesbatchidlog)     | Return submission log


See [livy documentation](https://github.com/cloudera/livy#rest-api) for complete details on request parameters and responses.

## Curl Examples

In the following curl requests, response headers are printed along with the JSON output to show the http response status codes.

### Submit a Spark job

Request:

```
curl -k -i -s \
-u "<username>:<password>" \
-H 'Content-Type: application/json' \
-d '{ "file":"local:/usr/iop/current/spark2-client/jars/spark-examples.jar", "className":"org.apache.spark.examples.SparkPi" }' \
"https://169.54.195.210:8443/gateway/default/livy/v1/batches"
```

Response:

```
HTTP/1.1 201 Created
Date: Fri, 14 Apr 2017 19:32:10 GMT
Set-Cookie: JSESSIONID=t3oc56v57se8ltlqp9s6jbj2;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:32:10 GMT
Date: Fri, 14 Apr 2017 19:32:10 GMT
Content-Type: application/json; charset=UTF-8
Location: /batches/34
Server: Jetty(9.2.16.v20160414)
Content-Length: 100

{
  "id": 34,
  "state": "starting",
  "appId": null,
  "appInfo": {
    "driverLogUrl": null,
    "sparkUiUrl": null
  },
  "log": []
}
```

### List all jobs

Request:

```
curl -k -s -i \
-u "<username>:<password>" \
https://169.54.195.210:8443/gateway/default/livy/v1/batches/
```

Response:

```
HTTP/1.1 200 OK
Date: Fri, 14 Apr 2017 19:34:26 GMT
Set-Cookie: JSESSIONID=ze5u1p3029kl16emanrfiu4c1;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:34:27 GMT
Date: Fri, 14 Apr 2017 19:34:27 GMT
Content-Type: application/json; charset=UTF-8
Server: Jetty(9.2.16.v20160414)
Content-Length: 10042

{
  "from": 0,
  "total": 10,
  "sessions": [
    {
      "id": 25,
      "state": "success",
      "appId": "application_1491850285904_0034",
      "appInfo": {
        "driverLogUrl": null,
        "sparkUiUrl": "http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0034/"
      },
      "log": [
        "\t diagnostics: [Fri Apr 14 18:45:04 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ; ",
        "\t ApplicationMaster host: N/A",
        "\t ApplicationMaster RPC port: -1",
        "\t queue: default",
        "\t start time: 1492195504118",
        "\t final status: UNDEFINED",
        "\t tracking URL: http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0034/",
        "\t user: clsadmin",
        "17/04/14 18:45:04 INFO ShutdownHookManager: Shutdown hook called",
        "17/04/14 18:45:04 INFO ShutdownHookManager: Deleting directory /tmp/spark-e75a8450-6a8f-47fa-b950-4433e4f93272"
      ]
    },
    ...
    ...
```

### Get job information

Request:

```
curl -k -s -i \
-u "<username>:<password>" \
https://169.54.195.210:8443/gateway/default/livy/v1/batches/34
```

Response:

```
HTTP/1.1 200 OK
Date: Fri, 14 Apr 2017 19:36:12 GMT
Set-Cookie: JSESSIONID=1olzabvjreeftw109ijzhyp8o;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:36:12 GMT
Date: Fri, 14 Apr 2017 19:36:12 GMT
Content-Type: application/json; charset=UTF-8
Server: Jetty(9.2.16.v20160414)
Content-Length: 1013

{
  "id": 34,
  "state": "success",
  "appId": "application_1491850285904_0043",
  "appInfo": {
    "driverLogUrl": null,
    "sparkUiUrl": "http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0043/"
  },
  "log": [
    "\t diagnostics: [Fri Apr 14 19:32:14 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ; ",
    "\t ApplicationMaster host: N/A",
    "\t ApplicationMaster RPC port: -1",
    "\t queue: default",
    "\t start time: 1492198334826",
    "\t final status: UNDEFINED",
    "\t tracking URL: http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0043/",
    "\t user: clsadmin",
    "17/04/14 19:32:14 INFO ShutdownHookManager: Shutdown hook called",
    "17/04/14 19:32:14 INFO ShutdownHookManager: Deleting directory /tmp/spark-358a7c98-4efc-4ba5-975b-3d9a2d1f8995"
  ]
}
```

### Get state

Request:

```
curl -k -s -i \
-u "<username>:<password>" \
https://169.54.195.210:8443/gateway/default/livy/v1/batches/34/state
```

Response:

```
HTTP/1.1 200 OK
Date: Fri, 14 Apr 2017 19:38:55 GMT
Set-Cookie: JSESSIONID=gvddwck8xi3x1p5e4pxdhmxrx;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:38:55 GMT
Date: Fri, 14 Apr 2017 19:38:55 GMT
Content-Type: application/json; charset=UTF-8
Server: Jetty(9.2.16.v20160414)
Content-Length: 27

{
  "id": 34,
  "state": "success"
}
```

### Get log

Request:

```
curl -k -s -i \
-u "<username>:<password>" \
https://169.54.195.210:8443/gateway/default/livy/v1/batches/34/log
```

Response:

```
HTTP/1.1 200 OK
Date: Fri, 14 Apr 2017 19:39:39 GMT
Set-Cookie: JSESSIONID=8zztd1bckcgh1l6vg6iaa22k8;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:39:39 GMT
Date: Fri, 14 Apr 2017 19:39:39 GMT
Content-Type: application/json; charset=UTF-8
Server: Jetty(9.2.16.v20160414)
Content-Length: 3313

{
  "id": 34,
  "from": 0,
  "total": 32,
  "log": [
    "17/04/14 19:32:11 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable",
    "17/04/14 19:32:12 WARN DomainSocketFactory: The short-circuit local reads feature cannot be used because libhadoop cannot be loaded.",
    "17/04/14 19:32:12 INFO RMProxy: Connecting to ResourceManager at enterprise-mn001.rocmg01.wdp-chs.ibm.com/10.155.161.236:8050",
    "17/04/14 19:32:13 INFO Client: Requesting a new application from cluster with 1 NodeManagers",
    "17/04/14 19:32:13 INFO Client: Verifying our application has not requested more than the maximum memory capability of the cluster (12288 MB per container)",
    "17/04/14 19:32:13 INFO Client: Will allocate AM container, with 1408 MB memory including 384 MB overhead",
    "17/04/14 19:32:13 INFO Client: Setting up container launch context for our AM",
    "17/04/14 19:32:13 INFO Client: Setting up the launch environment for our AM container",
    "17/04/14 19:32:13 INFO Client: Preparing resources for our AM container",
    "17/04/14 19:32:14 INFO Client: Source and destination file systems are the same. Not copying hdfs:/iop/apps/4.3.0.0-0000/spark2/spark2-iop-yarn-archive.tar.gz",
    "17/04/14 19:32:14 INFO Client: Uploading resource file:/tmp/spark-358a7c98-4efc-4ba5-975b-3d9a2d1f8995/__spark_conf__3057122678989565497.zip -> hdfs://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8020/user/clsadmin/.sparkStaging/application_1491850285904_0043/__spark_conf__.zip",
    "17/04/14 19:32:14 WARN Client: spark.yarn.am.extraJavaOptions will not take effect in cluster mode",
    "17/04/14 19:32:14 INFO SecurityManager: Changing view acls to: clsadmin",
    "17/04/14 19:32:14 INFO SecurityManager: Changing modify acls to: clsadmin",
    "17/04/14 19:32:14 INFO SecurityManager: Changing view acls groups to: ",
    "17/04/14 19:32:14 INFO SecurityManager: Changing modify acls groups to: ",
    "17/04/14 19:32:14 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users  with view permissions: Set(clsadmin); groups with view permissions: Set(); users  with modify permissions: Set(clsadmin); groups with modify permissions: Set()",
    "17/04/14 19:32:14 INFO Client: Submitting application application_1491850285904_0043 to ResourceManager",
    "17/04/14 19:32:14 INFO YarnClientImpl: Submitted application application_1491850285904_0043",
    "17/04/14 19:32:14 INFO Client: Application report for application_1491850285904_0043 (state: ACCEPTED)",
    "17/04/14 19:32:14 INFO Client: ",
    "\t client token: N/A",
    "\t diagnostics: [Fri Apr 14 19:32:14 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ; ",
    "\t ApplicationMaster host: N/A",
    "\t ApplicationMaster RPC port: -1",
    "\t queue: default",
    "\t start time: 1492198334826",
    "\t final status: UNDEFINED",
    "\t tracking URL: http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0043/",
    "\t user: clsadmin",
    "17/04/14 19:32:14 INFO ShutdownHookManager: Shutdown hook called",
    "17/04/14 19:32:14 INFO ShutdownHookManager: Deleting directory /tmp/spark-358a7c98-4efc-4ba5-975b-3d9a2d1f8995"
  ]
}
```

### Kill batch job

Request:

```
curl -k -s -i \
-u "<username>:<password>" \
-X DELETE \
https://169.54.195.210:8443/gateway/default/livy/v1/batches/34
```

Response:

```
HTTP/1.1 200 OK
Date: Fri, 14 Apr 2017 19:40:18 GMT
Set-Cookie: JSESSIONID=nsuwohcq4123gtcickaog46i;Path=/gateway/default;Secure;HttpOnly
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: rememberMe=deleteMe; Path=/gateway/default; Max-Age=0; Expires=Thu, 13-Apr-2017 19:40:18 GMT
Date: Fri, 14 Apr 2017 19:40:18 GMT
Content-Type: application/json; charset=UTF-8
Server: Jetty(9.2.16.v20160414)
Content-Length: 17

{
  "msg": "deleted"
}
```
