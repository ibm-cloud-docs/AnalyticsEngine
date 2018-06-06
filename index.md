---

copyright:
  years: 2017,2018
lastupdated: "2018-06-05"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introduction
With {{site.data.keyword.iae_full_notm}} you can create Apache Spark and Apache Hadoop clusters in minutes and customize these clusters by using scripts. You can work with data in IBM Cloud Object Storage, as well as integrate other IBM Watson services like {{site.data.keyword.DSX_short}} and Machine Learning.

You can define clusters based on your application's requirements choosing the appropriate software pack, version and size of the clusters.

You can deploy {{site.data.keyword.iae_full_notm}} service instances in the US South or United Kingdom regions. The {{site.data.keyword.iae_full_notm}} service is deployed in a data centre which is physically located in the chosen region.

## Software components of the cluster
You can create a cluster based on Hortonworks Data Platform 2.6.2 and 2.6.5. The following components are made available.

| HDP 2.6.2       | HDP 2.6.5        |
|---------------------|------------------------|
| Apache Spark 2.1.1 | Apache Spark 2.3.0 |
| Hadoop 2.7.3 | Hadoop 2.7.3|
| Apache Livy 0.3.0 | Apache Livy 0.3.0|
| Knox 0.12.0 | Knox 0.12.0|
| Ambari 2.5.2 | Ambari 2.6.2|
| Anaconda with Python 2.7.13 and 3.5.2 | Anaconda with Python 2.7.13 and 3.5.2|
| Jupyter Enterprise Gateway 0.8.0 | Jupyter Enterprise Gateway 0.8.0|
| HBase 1.1.2 &#42; | HBase 1.1.2 &#42;|
| Hive 1.2.1 &#42;&#42; | Hive 1.2.1 &#42;&#42;|
| Oozie 4.2.0 &#42; | Oozie 4.2.0 &#42;|
| Flume 1.5.2 &#42; | Flume 1.5.2 &#42;|
| Tez 0.7.0 &#42; | Tez 0.7.0 &#42;|
| Pig 0.16.0 &#42; | Pig 0.16.0 &#42;|
| Sqoop 1.4.6 &#42; | Sqoop 1.4.6 &#42;|
| Slider 0.92.0 &#42; | Slider 0.92.0 &#42;|
| Apache Phoenix 4.7 &#42; | Apache Phoenix 4.7 &#42;|


&#42; Available in the _AE 1.0/1.1 Spark and Hadoop_ pack only <br>
&#42;&#42; Available in the _AE 1.0/1.1 Spark and Hadoop_ and _AE 1.0/1.1 Spark and Hive_ packs only

## Hardware configuration

{{site.data.keyword.iae_full_notm}} supports two node sizes for spinning up clusters.

**Size: Default Node**

| Node Type | vCPU | Memory | HDFS Disks |
|---------|------------|-----------|-----------|
| Master Node | 4| 16 GB | NA |
| Data Node | 4| 16 GB | 2 x 300 GB |

**Size: Memory Intensive Node**

| Node Type | vCPU | Memory | HDFS Disks |
|---------|------------|-----------|-----------|
| Master Node | 32| 128 GB | NA |
| Data Node | 32| 128 GB | 3 x 300 GB |

## Operating System
The operating system used is Cent OS 7.
