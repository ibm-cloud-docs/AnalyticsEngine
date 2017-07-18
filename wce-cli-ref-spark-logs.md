---

copyright:
  years: 2017
lastupdated: "2017-07-13"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# spark‐logs
## Description

Gets the Spark job logs.

## Usage

```
bx ae spark-logs [--user <user>] [--password <password>] [--output <folder>] [--driver] [--executor] JOB-ID

bx ae spark-logs [--user <user>] [--password <password>] [--output <folder>] (--driver | --executor) --applicationID APPLICATION-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses

APPLICATION-ID is the YARN application ID for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses
```

## Options

Flag            | Description
--------------- | ---------------------------------------------------------------------------------------------------------------------------
--user          | A user with authority to access the cluster
--password      | A password for the cluster user
--driver        | Flag to indicate if the Spark driver logs should be included. If APPLICATION-ID provided, this flag or executor is required
--executor      | Flag to indicate if the Spark executor logs should be included. If APPLICATION-ID provided, this flag or driver is required
--outputDir     | Write output into this folder
--applicationID | Flag to indicate that an APPLICATION-ID will be supplied rather than a JOB-ID. If flag is not set, JOB-ID is expected.

## Examples

### Getting the submission log

```
$ bx ae spark-logs 2
User (clsadmin)>
Password>
Retreiving logs for job id '2'...
OK
Creating output file '/workspace/wdp-ae/jobid_2_submission.log'...
OK
Writing...
OK
```

### Getting the driver log

Using the `--driver` flag will retrieve the spark driver logs.

```
$ bx ae spark-logs --driver --outputDir driverlogs 2
User (clsadmin)>
Password>
Retreiving YARN app id for job id '2'...
OK
App id: application_1491850285904_0003
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: enterprise-dn001.rocmg01.wdp-chs.ibm.com
Locating spark driver logs in HDFS...
OK
Downloading log file 'enterprise-dn001.rocmg01.wdp-chs.ibm.com_45454'...
OK
Parsing raw log TFile...
Creating output file 'driverlogs/jobid_2_container_1491850285904_0003_01_000001_driver_stderr.log'...
OK
Writing...
OK
Creating output file 'driverlogs/jobid_2_container_1491850285904_0003_01_000001_driver_stdout.log'...
OK
Writing...
OK
```

### Getting the executor log

Using the `--executor` flag will retrieve the spark executor logs.

```
$ bx ae spark-logs --executor --outputDir executorlogs 2
User (clsadmin)>
Password>
Retreiving YARN app id for job id '2'...
OK
App id: application_1491850285904_0003
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: enterprise-dn001.rocmg01.wdp-chs.ibm.com
Locating spark driver logs in HDFS...
OK
Downloading log file 'enterprise-dn001.rocmg01.wdp-chs.ibm.com_45454'...
OK
Parsing raw log TFile...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000003_executor_stderr.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000003_executor_stdout.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000002_executor_stderr.log'...
OK
Writing...
OK
Creating output file 'executorlogs/jobid_2_container_1491850285904_0003_01_000002_executor_stdout.log'...
OK
Writing...
OK
```

### Getting the driver or executor logs by using the YARN application ID

Because the job ID is short lived, you can also obtain logs that use the YARN application ID by using the `--applicationID` flag. The applicationID value is printed to the console when the job is submitted in synchronous mode (`spark-submit` without `--asynchronous` flag).

```
$ bx ae spark-logs --driver --applicationID application_1491850285904_0003
User (clsadmin)>
Password>
Retreiving app state in YARN...
OK
State: FINISHED
Retreiving spark driver info...
OK
Spark driver ContainerID: container_1491850285904_0003_01_000001
Spark driver Node: enterprise-dn001.rocmg01.wdp-chs.ibm.com
Locating spark driver logs in HDFS...
OK
Downloading log file 'enterprise-dn001.rocmg01.wdp-chs.ibm.com_45454'...
OK
Parsing raw log TFile...
Creating output file '/workspace/wdp-ae/appid_application_1491850285904_0003_container_1491850285904_0003_01_000001_driver_stderr.log'...
OK
Writing...
OK
Creating output file '/workspace/wdp-ae/appid_application_1491850285904_0003_container_1491850285904_0003_01_000001_driver_stdout.log'...
OK
Writing...
OK
```
