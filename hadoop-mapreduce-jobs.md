---

copyright:
  years: 2017, 2019
lastupdated: "2018-03-06"

subcollection: AnalyticsEngine
---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Running Hadoop MapReduce jobs
{: #run-hadoop-jobs}

You can process large volumes of data in parallel by running the analysis processes as jobs.

## Prerequisites
{: #mapreduce-prereqs}

You need the cluster user credentials, the SSH and oozie_rest endpoint details from the service credentials of your service instance.

## Analyzing data by opening the SSH connection

You can work with your data in a Hadoop MapReduce program by opening the SSH connection to the cluster through a Yarn command.

You must run `TeraGen` to generate random data that can be used as input data for subsequent data analysis:

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar  \
teragen  1000000   /user/clsadmin/teragen/test1G
```
{: codeblock}

## Compressing output from large workloads

If you are running MapReduce jobs with large workloads, consider enabling compressing the output to reduce the size of the intermediate data. To enable compression, set the `mapreduce.map.output.compress` property to `true` in your command string.

You must run the TeraGen (TeraGen generates the input for TeraSort) sample code in the previous section before you run the following sample code to compress the data using `TeraSort`:

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort \
  -D mapreduce.map.output.compress=true  \
  /user/clsadmin/teragen/test1G /user/clsadmin/terasort/test1Gsort
```
{: codeblock}

## Running wordcount on data in {{site.data.keyword.cos_short}}

The following example shows running a wordcount Jarn application in {{site.data.keyword.cos_short}}:
```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount cos://mybucket.myprodservice/input cos://mybucket.myprodservice/wordcount/output
```

For information on configuring the cluster to work with {{site.data.keyword.cos_full_notm}}, see [Working with  {{site.data.keyword.cos_short}}](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-config-cluster-cos).

## Learn more
{: #learn-more-mapreduce-jobs}

[Submitting MapReduce jobs with Oozie](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-working-with-oozie).
