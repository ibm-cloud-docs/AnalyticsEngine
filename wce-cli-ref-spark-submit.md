---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# spark‐submit
## Description

Submits a Spark job to the {{site.data.keyword.iae_full_notm}} cluster.

## Usage

```
bx ae spark-submit [--user <user>] [--password <password>] [--proxyUser <user>] [--className <main-class>] [--args <arg>]... [--jars <jar>]... [--pyFiles <file>]... [--files <file>]... [--driverMemory <memory>] [--driverCores <number>] [--executorMemory <memory>] [--executorCores <number>] [--numExecutors <number>] [--archives <archive>]... [--queue <queue>] [--name <value>] [--conf <key=val>]... [--asynchronous] FILE
FILE is the file containing the application to execute
```

## Options

Flag             | Description
---------------- | -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
--user           | A user with authority to access the cluster
--password       | A password for the cluster user
--proxyUser      | User to impersonate when running the job
--className      | Application Java/Spark main class
--args           | Command line arguments for the application. Argument can be repeated multiple times.
--jars           | Jars to be used in this Job. Argument can be repeated multiple times.
--pyFiles        | Python files to be used in this job. Argument can be repeated multiple times.
--files          | Files to be used in this job. Argument can be repeated multiple times.
--driverMemory   | Amount of memory to use for the driver process
--driverCores    | Number of cores to use for the driver process
--executorMemory | Amount of memory to use per executor process
--executorCores  | Number of cores to use for each executor
--numExecutors   | Number of executors to launch for this session
--archives       | Archives to be used in this session. Argument can be repeated multiple times.
--queue          | The name of the YARN queue to which submitted
--name           | The name of this session
--conf           | Spark configuration property. Provide a single name=value pair. Argument can be repeated multiple times.
--asynchronous   | Execute spark submit and return immediately without waiting for an application ID
--upload         | If set, all file related arguments will be treated as local file system references and will be uploaded to HDFS. Once uploaded, spark submit will refer to the files in their HDFS locations. If this is set, any URI references such as `<scheme>://<host:port>/<path>` will be treated as an error.

## Examples

### Submitting a Spark job - app on cluster

In this example `/user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar` is a file on HDFS. The example shows submitting a Spark job if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ bx ae spark-submit --className org.apache.spark.examples.SparkPi /user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
Job ID '4'
Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK
Job ID '4'
Application ID 'application_1491850285904_0005'
Done
```

When HDFS `host` and `port` is known, HDFS file scheme can be used in `FILE` argument. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`.

```
$ bx ae spark-submit --className org.apache.spark.examples.SparkPi hdfs://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8020/user/clsadmin/jobs/spark-examples_2.11-2.1.0.jar
```

### Submitting a local Spark job or app (not on cluster)

The Spark job `spark-examples_2.11-2.1.0.jar` is located on the user's local system remote from the cluster. By using the `--upload` flag the Spark job is copied to a unique directory in the user's HDFS home directory. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`.

```
$ bx ae spark-submit --className org.apache.spark.examples.SparkPi --upload spark-examples_2.11-2.1.0.jar
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
Job ID '2'
Waiting for job to return application ID. Will check every 10 seconds, and stop checking after 2 minutes. Press Control C to stop waiting.
If you would like to repeat this request without re-uploading your files again, please use:


bx ae spark-submit --user clsadmin --password <YOUR PASSWORD HERE> --className org.apache.spark.examples.SparkPi  hdfs://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8020/user/clsadmin/cli/0fdd1caf-a21f-4a18-970c-e40255e8f0ad/spark-examples_2.11-2.1.0.jar


Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK
Job ID '2'
Application ID 'application_1491850285904_0003'
Done
```

In this example, the Spark job is copied to `hdfs://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8020/user/clsadmin/cli/0fdd1caf-a21f-4a18-970c-e40255e8f0ad/spark-examples_2.11-2.1.0.jar`. This location can be used to run the job again without having to uploading it again. The command output contains this information.

### Submitting a job without waiting for the application ID

By default, the `spark-submit` command waits (polls) for  status to get the YARN application ID before returning.  If you want to submit a job and not wait for the application ID, you can provide the `--asynchronous` option. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`

```
$ bx ae spark-submit --asynchronous --upload pi.py
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
If you would like to repeat this request without re-uploading your files again, please use:


bx ae spark-submit --user clsadmin --password <YOUR PASSWORD HERE>  --asynchronous hdfs://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8020/user/clsadmin/cli/02eb0c74-2c31-41ad-ae63-4d09fa8096a1/pi.py


Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK
Job ID '8'
Application ID ''
```
