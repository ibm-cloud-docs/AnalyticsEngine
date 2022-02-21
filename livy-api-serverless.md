---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-09"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}

# Livy batch APIs
{: #livy-api-serverless}

Livy batches API is a REST interface for submitting Spark batch jobs. This interface is very similar to the open source Livy REST interface (see [Livy](https://github.com/cloudera/livy)) except for a few limitations which are described in the following topic.

The open source Livy batches log API to retrieve log lines from a batch job is not supported. The logs are added to the {{site.data.keyword.cos_full_notm}} bucket that was referenced as the service instance `"instance_home"`. At a later time during the beta release, the logs can be forwarded to {{site.data.keyword.la_full_notm}}.
{: note}

Gets the log lines from this batch.

## Submitting Spark batch jobs

To submit a Spark batch job by using the Livy batches API, enter:

```sh
curl \
-H 'Authorization: Bearer <TOKEN>' \
-H 'Content-Type: application/json' \
-d '{ "file": "/ cos://<application-bucket-name>.<cos-reference-name>/my_spark_application.py"
", \
"conf": { \
      "spark.hadoop.fs.cos.<cos-reference-name>.endpoint": "https://s3.us-south.cloud-object-storage.appdomain.cloud", \
      "spark.hadoop.fs.cos.<cos-reference-name>.access.key": "<access_key>", \
      "spark.hadoop.fs.cos.<cos-reference-name>.secret.key": "<secret_key>", \
      "spark.app.name": "MySparkApp" \
      } \
}' \
-X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches
```
{: pre}

Request body for a submitted batch job using the Livy batches API:

| Name | Description | Type        |
|------|-------------|-------------|
| `file` | File containing the application to execute | string (required) |
| `className`	| Application Java/Spark main class | string |
| `args` | Command line arguments for the application | list of string |
| `jars` | jars to be used in this session | list of string |
| `pyFiles`	| Python files to be used in this session | list of string |
| `files` | files to be used in this session | list of string |
| `driverMemory` | Amount of memory to use for the driver process | string |
| `driverCores`	| Number of cores to use for the driver process | int |
| `executorMemory` | Amount of memory to use per executor process | string |
| `executorCores`	| Number of cores to use for each executor | int |
| `numExecutors` | Number of executors to launch for this session | int |
| `name` | The name of this session | string |
| `conf` | Spark configuration properties | map of key=val |
{: caption="Table 1. Request body for batch jobs" caption-side="top"}


The `proxyUser`, `archives` and `queue` properties are not supported in the request body although they are supported in  the open source Livy REST interface.
{: note}

Response body of a submitted batch job using the Livy batches API:

| Name | Description | Type        |
|------|-------------|-------------|
| `id` |	The batch ID |	int |
| `appId` | The Spark application ID | string |
| `appInfo` | Detailed application information |	map of key=val |
| `state` | State of submitted batch job | string |
{: caption="Table 2. Response body of a submitted batch job" caption-side="top"}


## Examples using the Livy API

The following sections show you how to use the Livy batches APIs.

### Submit a batch job with job file in  {{site.data.keyword.cos_full_notm}}

To submit a batch job where the job file is located in an {{site.data.keyword.cos_full_notm}} bucket, enter:
```sh
curl -i -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN" -d @livypayload.json
```
{: pre}

The endpoint to your {{site.data.keyword.cos_full_notm}} instance in the payload JSON file should be the public endpoint.
<!--
The endpoint to your {{site.data.keyword.cos_full_notm}} instance in the payload JSON file should be the `direct` endpoint. You can find the `direct` endpoint to your {{site.data.keyword.cos_full_notm}} instance on the {{site.data.keyword.Bluemix_short}} dashboard by selecting cross regional resiliency, the location, which should preferably match the location of your {{site.data.keyword.iae_short}} instance, and then clicking on your service instance. You can copy the direct endpoint from the **Endpoints** page.  -->

Sample payload:
```json
{
  "file": "cos://<bucket>.mycos/wordcount.py",
  "className": "org.apache.spark.deploy.SparkSubmit",
  "args": ["/opt/ibm/spark/examples/src/main/resources/people.txt"],
  "conf": {
    "spark.hadoop.fs.cos.mycos.endpoint": "https://s3.us-south.cloud-object-storage.appdomain.cloud",
    "spark.hadoop.fs.cos.mycos.access.key": "XXXX",
    "spark.hadoop.fs.cos.mycos.secret.key": "XXXX",
    "spark.app.name": "MySparkApp"
    }
}    
```
{: codeblock}

Sample response:
```json
{"id":13,"app_info":{},"state":"not_started"}
```

### Submit batch job with job file on local disk

To submit a batch job where the job file is located on a local disk enter:
```sh
curl -i -X POST https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN" -d @livypayload.json
```
{: pre}

Sample payload:
```json
{
  "file": "/opt/ibm/spark/examples/src/main/python/wordcount.py",
  "args": ["/opt/ibm/spark/examples/src/main/resources/people.txt"],
  "className": "org.apache.spark.deploy.SparkSubmit"
}
```
{: codeblock}

Sample response:
```json
{"id":15,"app_info":{},"state":"not_started"}
```

The `SparkUiUrl` property in response will have a non-null value when the UI is available for serverless Spark instance.
{: note}

### List the details of a job

To list the job details for a particular Spark batch job enter:
```sh
curl \
-H 'Authorization: Bearer <TOKEN>' \
-H 'Content-Type: application/json' \
-X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/<batch-id>
```
{: pre}

The response body for listing the job details:

| Name | Description | Type        |
|------|-------------|-------------|
| `id` |	The batch ID |	int |
| `appId` | The Spark application ID | string |
| `appInfo` | Detailed application information |	map of key=val |
| `state` | State of submitted batch job | string |
{: caption="Table 3. Response body for listing job details" caption-side="top"}


An example:
```sh
curl -i -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/43f79a18-768c-44c9-b9c2-b19ec78771bf/livy/batches/14 -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN"
```
{: pre}

Sample response:
```json
{
 "id": 14,
 "appId": "app-20201213175030-0000",
 "appInfo": {
   "sparkUiUrl": null
 },
 "state": "success"
}
```

The `SparkUiUrl` property in response will have a non-null value when the UI is available for serverless Spark instance.
{: note}

### Get job state

To get the state of your submitted job enter:
```sh
curl \
-H 'Authorization: Bearer <TOKEN>' \
-H 'Content-Type: application/json' \
-X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/<batch-id>/state
```
{: pre}

The response body for getting the state of the batch job:

| Name | Description | Type        |
|------|-------------|-------------|
| `id` |	The batch ID |	int |
| `state` | State of submitted batch job | string |
{: caption="Table 4. Response body for getting state of batch job" caption-side="top"}


For example:
```sh
curl -i -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/43f79a18-768c-44c9-b9c2-b19ec78771bf/livy/batches/14/state -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN"
```
{: pre}

Sample response:
```json
{
	"id": 14,
	"state": "success"
}
```

### List all submitted jobs

To list all of the submitted Spark batch jobs enter:
```sh
curl \
-H 'Authorization: Bearer <TOKEN>' \
-H 'Content-Type: application/json' \
-X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches
```
{: pre}

The `from` and `size` properties are not supported in the request body although they are supported in the open source Livy REST interface.
{: note}

The response body for listing all submitted Spark batch jobs:

| Name | Description | Type        |
|------|-------------|-------------|
| `from` | The start index of the Spark batch jobs that are retrieved |	int |
| `total` | The total number of batch jobs that are retireved | int |
| `sessions`| The details for each batch job in a session | list |
{: caption="Table 5. Response body for listing all submitted batch jobs" caption-side="top"}


For example:
```sh
curl -i -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/43f79a18-768c-44c9-b9c2-b19ec78771bf/livy/batches -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN"
```
{: pre}

Sample response:
```json
{
  "from": 0,
  "sessions": [{
    "id": 13,
		"appId": "app-20201203115111-0000",
		"appInfo": {
			"sparkUiUrl": null
		},
		"state": "success"
    },
    {
		"id": 14,
		"appId": "app-20201213175030-0000",
		"appInfo": {
			"sparkUiUrl": null
		},
		"state": "success"
	}],
	"total": 2
}
```

The `SparkUiUrl` property in response will have a non-null value when the UI is available for serverless Spark instance.
{: note}

### Delete a job

To delete a submitted batch job enter:
```sh
curl \
-H 'Authorization: Bearer <TOKEN>' \
-H 'Content-Type: application/json' \
-X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/<batch-id>
```
{: pre}

For example:
```sh
curl -i -X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/43f79a18-768c-44c9-b9c2-b19ec78771bf/livy/batches/14 -H 'content-type: application/json' -H "Authorization: Bearer $TOKEN"
```
{: pre}

Sample response:
```json
{
	"msg": "deleted"
}
```
