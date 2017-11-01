---

copyright:
  years: 2017
lastupdated: "2017-09-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Introduction
With IBM Analytics Engine you can create Apache Spark and Apache Hadoop clusters in minutes and customize these clusters by using scripts. You can work with data in IBM Cloud Object Storage, as well as integrate other Watson Data Platform services like IBM Data Science Experience and Machine Learning.

You can define clusters based on your application's requirements choosing the appropriate software pack, version and size of the clusters

## Software components of the cluster
The cluster is based on Hortonworks Data Platform 2.6.2. The following components are made available.

Apache Spark 2.1.1
Hadoop 2.7.3<br>
Jupyter Enterprise Gateway 0.5.0<br>
Apache Livy 0.3.0<br>
Knox 0.12.0<br>
Ambari 2.5.2<br>
Anaconda with Python 2.7.13 and 3.5.2 <br>
HBase 1.1.2 &#42; <br>
Hive 1.2.1 &#42;<br>
Oozie 4.2.0 &#42;<br>
Flume 1.5.2 &#42; <br>

&#42;Available in _AE 1.0 Hadoop and Spark pack_ only

## Hardware configuration

IBM Analytics Engine supports two node sizes for spinning up clusters.<br>
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
