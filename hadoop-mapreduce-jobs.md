---

copyright:
  years: 2017,2018
lastupdated: "2018-03-06"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Running Hadoop MapReduce jobs

**Prerequisite**: Obtain the cluster user credentials, SSH and oozie_rest end point details from the service credentials of your service instance.

## Analyzing data by opening the SSH connection

You can work with your data by analyzing the data with a Hadoop MapReduce program by opening the SSH connection to the cluster through a Yarn command.

### Example with TeraGen

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar  \
teragen  1000000   /user/clsadmin/teragen/test1G
```
{: codeblock}

## Compressing output from large workloads

If you are running MapReduce jobs with large workloads, consider enabling compression for the output to reduce the size of the intermediate data. To enable such compression, set the `mapreduce.map.output.compress` property to `true` in your command string.

You must run the TeraGen sample code in the previous section before you run the following TeraSort sample code.

### Example with TeraSort

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort \
  -D mapreduce.map.output.compress=true  \
  /user/clsadmin/teragen/test1G /user/clsadmin/terasort/test1Gsort
```
{: codeblock}

## Running wordcount on data stored in S3-based object stores

### Example running Wordcount using COS
```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount cos://mybucket.myprodservice/input cos://mybucket.myprodservice/wordcount/output
```

For more information on configuring the cluster to work with S3 object stores, see [Configuring clusters to work with IBM COS S3 object stores](./configure-COS-S3-object-storage.html).

## Learn more

[Submitting MapReduce jobs with Oozie](./working-with-oozie.html).
