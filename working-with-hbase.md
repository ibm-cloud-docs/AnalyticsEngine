---

copyright:
  years: 2017
lastupdated: "2018-01-09"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with HBase
Apache HBase is a column-oriented database management system that runs on top of HDFS and is often used for sparse data sets. Unlike relational database systems, HBase does not support a structured query language like SQL.

HBase applications are written in Java, much like a typical MapReduce application. HBase allows many attributes to be grouped into column families so that the elements of a column family are all stored together. This approach is different from a row-oriented relational database, where all columns of a row are stored together.

**Note:** HBase and Apache Phoenix are only available in the Analytics Engine Hadoop package.

## Accessing HBase through the HBase shell
To work with HBase, you need your cluster user credentials and the SSH credentials. You can get this information from the service credentials of your Analytics Engine service instance.

To connect to the HBase server:
1.	Issue the SSH command to access the cluster.
2.	Launch the HBase CLI by executing the following command:
```
hbase shell```
3.	Use the regular shell commands for HBase to create, list, and read tables.

**Restriction:** The HBase REST interface through Knox is not supported.

For further information on HBase and its features refer to [Apache HBase](https://hortonworks.com/apache/hbase/).

## Accessing Phoenix through client tools
Apache Phoenix enables SQL-based OLTP and operational analytics for Apache Hadoop using Apache HBase as its backing store. It is based on the [Avatica](https://calcite.apache.org/avatica) component of [Apache Calcite](https://calcite.apache.org/). The Phoenix Query Server is comprised of a Java server that manages the Phoenix connections on the client’s behalf. The client implementation is currently a JDBC driver with few dependencies. It supports two transport mechanisms currently: JSON and Protocol Buffers (PROTOBUF). The query server on the IBM Analytics Engine cluster uses PROTOBUF serialization by default, which is more efficient compared to JSON serialization.

Apache Phoenix enables you to interact with HBase using SQL through Phoenix client tools like `sqlline.py` and `psql.py`.

### Using the SQLLine client
`Sqlline.py` is a tool for running queries interactively or can be used to run a .sql file. To start the SQLLine client:

1.	Issue the SSH command to access the cluster.
2.	Navigate to `/usr/hdp/current/phoenix-client/bin`
3.	Launch `sqlline.py` in one of the following ways:

 -	By entering: `./sqlline.py` (which will launch a query prompt)
 - By entering: `./sqlline.py /local_path_to_folder/createTable.sql`

### Using the psql client
`Psql.py` is a client tool for loading CSV formatted data on your local file system by using the psql command.

To load data via `psql.py`:

1.	Issue the SSH command to access the cluster.
2.	Navigate to `/usr/hdp/current/phoenix-client/bin`.
3.	Run `psql.py`:
```
	./psql.py /local_path_to_folder/createTable.sql /local_path_to_folder/data.csv```

### Bulk Loading by starting a MapReduce job
Phoenix provides Hadoop libraries for the MapReduce-based bulk loading tool for CSV and JSON formatted data on HDFS.

The following example shows loading a table using a DDL statement:
```
CREATE TABLE example (
    id bigint not null,
    m.fname varchar(50),
    m.lname varchar(50)
    CONSTRAINT pk PRIMARY KEY (id))

```
To launch the MapReduce loader, use the following Hadoop command with the Phoenix client jar:
 - For CSV data, use:  
 ```
  	HADOOP_CLASSPATH=/usr/hdp/current/hbase-master/lib/hbase-protocol.jar:/usr/hdp/current/hbase-master/conf hadoop jar /usr/hdp/current/phoenix-client/phoenix-<VERSION>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table EXAMPLE --input /user/clsadmin/data.csv```

 - For JSON data, use:

 ```
 	HADOOP_CLASSPATH=/usr/hdp/current/hbase-master/lib/hbase-protocol.jar:/usr/hdp/current/hbase-master/conf hadoop jar /usr/hdp/current/phoenix-client/phoenix-<VERSION>-client.jar org.apache.phoenix.mapreduce.JsonBulkLoadTool --table EXAMPLE --input /user/clsadmin/data.json ```

  **Note:** The JSON data file must be of Hadoop JSON input format which has one JSON record per line. The following example shows the Hadoop JSON input format
  ```
  {"id":123,"fname":"paddy","lname":"ashok"}
  {"id":456,"fname":"madhu","lname":"jolly"}```

For more information on bulk loading, see [Apache Phoenix Bulk Data Loading](https://phoenix.apache.org/bulk_dataload.html).

### SQL sample statements
The following examples show useful SQL statements for the Phoenix Query Server:

 - Example of the CREATE TABLE statement:

 ```CREATE TABLE my_schema.my_table (id BIGINT not null primary key, date);
```


 - Example of the UPSERT statement. Note that this statement inserts a value if it does not exist, or updates the value in the table if it  already exists.

 ```UPSERT INTO TEST VALUES('foo','bar',3);
```


- Example of the SELECT statement:

  ```SELECT * FROM TEST LIMIT 1000;
```


- Example of the DELETE statement:

  ```DELETE FROM TEST WHERE ID=123;
```

- Examples of the DROP TABLE statement:

  ```
 DROP TABLE my_schema.my_table;

 DROP TABLE my_schema.my_table CASCADE;
```

For the complete list of supported SQL statements, see [Apache Phoenix](https://phoenix.apache.org/).

## Accessing Phoenix through JDBC via the Knox Gateway

You can also access Apache Phoenix securely via the Knox Gateway. The cluster user credentials and the phoenix_jdbc endpoint are required as well as the [Phoenix 4.9 client Java libraries](https://archive.apache.org/dist/phoenix/apache-phoenix-4.9.0-HBase-1.1/bin/apache-phoenix-4.9.0-HBase-1.1-bin.tar.gz), which must be added to the Java classpath.

The following code snippet for a JDBC client program shows you how to connect to Apache Phoenix through the Knox Gateway:

```
String phoenix_jdbc_url = “jdbc:phoenix:thin:url=https://chs-XXXXX-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/avatica;authentication=BASIC;serialization=PROTOBUF”;
Connection conn;
Class.forName("org.apache.phoenix.jdbc.PhoenixDriver");
Properties props = new Properties();
props.setProperty("avatica_user", "clsadmin");
props.setProperty("avatica_password", "XXXXX");
DriverManager.getConnection(phoenix_jdbc_url, props);
```
**Restrictions:**
- The Apache Phoenix 4.7 client libraries that are  shipped with HDP 2.6.2 do not support the HTTPS protocol. This is a known [Knox issue](https://issues.apache.org/jira/browse/KNOX-893) and a workaround is to use the Phoenix 4.9 client libraries instead.
- The tool `sqlline-thin.py` (v1.1.8), which is shipped with HDP 2.6.2 does not support the HTTPS protocol either because of the same known issue mentioned for the Apache Phoenix 4.7 client libraries.
-	The Hortonworks Phoenix ODBC driver currently does not support access to the Apache Phoenix server through the  Knox Gateway.
