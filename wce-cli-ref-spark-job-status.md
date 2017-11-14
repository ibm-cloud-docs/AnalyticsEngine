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

# spark‐job‐status
## Description

Retrieves the status of a Spark job on the IBM Analytics Engine cluster.

## Usage

```
bx ae spark-job-status [--user <user>] [--password <password>] JOB-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses
```

## Options

Flag       | Description
---------- | ----------------------------------------------------
--user     | A user with authority to get the version information
--password | The password for the selected user

## Examples

To view the status of a job:

```
$ bx ae spark-job-status 3
User (clsadmin)>
Password>
Contacting endpoint 'https://169.54.195.210:8443'...
Finished contacting endpoint 'https://169.54.195.210:8443'
OK
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0004/'
Log lines '     diagnostics: [Tue Apr 11 17:33:16 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ;
     ApplicationMaster host: N/A
     ApplicationMaster RPC port: -1
     queue: default
     start time: 1491931996616
     final status: UNDEFINED
     tracking URL: http://enterprise-mn001.rocmg01.wdp-chs.ibm.com:8088/proxy/application_1491850285904_0004/
     user: clsadmin
17/04/11 17:33:16 INFO ShutdownHookManager: Shutdown hook called
17/04/11 17:33:16 INFO ShutdownHookManager: Deleting directory /tmp/spark-6fbb8870-edea-47e0-9e2b-be2901b4bc2f'
```

To view the status of all jobs, see command [spark-job-statuses](./staging/wce-cli-ref-spark-job-statuses.html).
