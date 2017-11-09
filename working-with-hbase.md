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

# Working with HBase

Apache HBase is a column-oriented database management system that runs on top of HDFS and is often used for sparse data sets. Unlike relational database systems, HBase does not support a structured query language like SQL. HBase applications are written in Java, much like a typical MapReduce application. HBase allows many attributes to be grouped into column families so that the elements of a column family are all stored together. This approach is different from a row-oriented relational database, where all columns of a row are stored together.

## Accessing the HBase server
To work with HBase, you need your cluster user credentials and the SSH credentials. You can get this information from the service credentials of your Analytics Engine service instance.

To connect to the HBase server:

1. Issue the SSH command to access the cluster.
2. Launch the HBase CLI by executing the following command: `hbase shell`.
3. Use the regular shell commands for HBase to create, list, and read tables.

Restriction: The HBase REST interface through Knox is not supported.

For further information on HBase and its features refer to [Apache HBase](https://hortonworks.com/apache/hbase/).
