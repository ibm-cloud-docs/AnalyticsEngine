---

copyright:
  years: 2017
lastupdated: "2017-07-27"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# spark‐job‐cancel
## Description

Cancels a Spark job submitted on the IBM Analytics Engine cluster.

## Usage

```
bx ae spark-job-cancel [--user <user>] [--password <password>] JOB-ID

JOB-ID is the identifier for the job. This value is returned from spark-submit, spark-job-status or spark-job-statuses
```

## Options

Flag       | Description
---------- | ----------------------------------------------------
--user     | A user with authority to get the version information
--password | The password for the selected user

## Examples

```
$ bx ae spark-job-cancel 10
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net:8443'
OK
deleted
```
