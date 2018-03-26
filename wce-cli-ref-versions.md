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

# versions
## Description

Gets the versions of the services running in the {{site.data.keyword.iae_full_notm}} cluster.

## Usage

```
bx ae versions [--user <user>] [--password <password>] [--serviceDetails]
```

## Options

| Flag | Description |
| --- | --- |
|--user| A user with authority to get the version information|
|--password| The password for the selected user |
|--serviceDetails|Provides information about the services installed within the cluster|

## Examples

### Viewing the IOP stack version
```
$ bx ae versions
Current user is 'clsadmin'
Password>
OK
Combined hadoop spark cluster version: BigInsights-4.3
```

### Viewing all service versions

```
$ bx ae versions --serviceDetails
Current user is 'clsadmin'
Password>
OK
Service Name     Service Version
Ambari Infra     0.1.0
Ambari Metrics   0.1.0
Flume            1.7.0
HBase            1.2.4
HDFS             2.7.3
Hive             1.2.1
JNBG             0.2.0
Kafka            0.10.1.0
Kerberos         1.10.3
Knox             0.11.0
Livy             0.3.0
Log Search       0.5.0
MapReduce2       2.7.3
Oozie            4.3.0
Pig              0.16.0
R4ML             0.8.0
Ranger           0.6.2
Ranger KMS       0.6.2
Slider           0.91.0
Solr             6.3.0
Spark2           2.1.0
Sqoop            1.4.6
SystemML         0.13.0
Titan            1.0.0
YARN             2.7.3
ZooKeeper        3.4.6
```
