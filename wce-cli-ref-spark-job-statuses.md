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

# spark‐job‐statuses
## Description

Retrieves the status of all Spark jobs on the IBM Analytics Engine cluster.

## Usage

```
bx ae spark-job-statuses [--user <user>] [--password <password>] [--includeSubmissionLogs]
```

## Options

Flag                    | Description
----------------------- | ------------------------------------------------------------------
--user                  | A user with authority to get the version information
--password              | The password for the selected user
--includeSubmissionLogs | Flag to indicate if the logs should be included in the status list

## Examples

To view the status of all jobs:

```
$ bx ae spark-job-statuses
User (clsadmin)>
Password>
Start index to fetch jobs [Optional: Press enter for no value]>
Number of jobs [Optional: Press enter for no value]>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK

Start index of jobs '0'
Total jobs active '1'
Displaying '1' jobs:

ID '3'
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://chs-xxx-xxx-mn002.bi.services.us-south.bluemix.net:8088/proxy/application_1491850285904_0004/'
```

## To show the logs with the status of all jobs:

```
$ bx ae spark-job-statuses --includeSubmissionLogs
User (clsadmin)>
Password>
Start index to fetch jobs [Optional: Press enter for no value]>
Number of jobs [Optional: Press enter for no value]>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK

Start index of jobs '0'
Total jobs active '1'
Displaying '1' jobs:

ID '3'
App ID 'application_1491850285904_0004'
State 'success'
App Info 'driverLogUrl' = ''
App Info 'sparkUiUrl' = 'http://chs-xxx-xxx-mn002.bi.services.us-south.bluemix.net:8088/proxy/application_1491850285904_0004/'
Log lines '     diagnostics: [Tue Apr 11 17:33:16 +0000 2017] Application is Activated, waiting for resources to be assigned for AM.  Details : AM Partition = <DEFAULT_PARTITION> ; Partition Resource = <memory:20480, vCores:4> ; Queue's Absolute capacity = 100.0 % ; Queue's Absolute used capacity = 0.0 % ; Queue's Absolute max capacity = 100.0 % ;
     ApplicationMaster host: N/A
     ApplicationMaster RPC port: -1
     queue: default
     start time: 1491931996616
     final status: UNDEFINED
     tracking URL: http://chs-xxx-xxx-mn002.bi.services.us-south.bluemix.net:8088/proxy/application_1491850285904_0004/
     user: clsadmin
17/04/11 17:33:16 INFO ShutdownHookManager: Shutdown hook called
17/04/11 17:33:16 INFO ShutdownHookManager: Deleting directory /tmp/spark-6fbb8870-edea-47e0-9e2b-be2901b4bc2f'
```
