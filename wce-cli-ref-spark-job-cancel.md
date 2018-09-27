---

copyright:
  years: 2017,2018
lastupdated: "2018-09-27"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# spark‐job‐cancel
## Description

Cancels a Spark job submitted on the {{site.data.keyword.iae_full_notm}} cluster.

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
The following example shows how to cancel a Spark job if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

```
$ bx ae spark-job-cancel 10
User (clsadmin)>
Password>
Contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'...
Finished contacting endpoint 'https://chs-xxx-xxx-mn001.us-south.ae.appdomain.cloud:8443'
OK
deleted
```
