---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Hive 

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL. 

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an IBM Analytics Engine service, Hive commands can be executed through the Beeline client and by default, the Hive uses Tez as its execution engine. Note that Hive is not available in the Analytics Engine Spark package. 

## Prerequisites
To work with Hive, you need your cluster user credentials and the ssh and hive_jdbc end point details. You can get this information from the service credentials of your Analytics Engine service instance.

## Connecting to the Hive server

Connect to the Hive server by using with beeline client. 

Issue the following SSH command to the cluster:

```
ssh clsadmin@chs-xxxxx-mn003.bi.services.us-south.bluemix.net
beeline -u 'jdbc:hive2://chs-xxxxx-mn001.bi.services.us-south.bluemix.net:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive' -n clsadmin -p **********
```

The following examples show useful HiveQL statements.

- Example of the CREATE TABLE statement:

	`CREATE TABLE docs (line STRING);`

- Example of the LOAD statement:

	`LOAD DATA INPATH 'path_to_hdfs_data.txt' OVERWRITE INTO TABLE docs;`

- Example of the SELECT statement:

	`SELECT * from doc;`

- Example of a CREATE TABLE statement with data in COS object store:

	`CREATE EXTERNAL TABLE s3aTable( no INT, name STRING) 
	ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
	LOCATION 's3a://mybucket/myhivedir';` 


## Changing the Hive execution engine

To change the hive execution engine from Tez to MR, run the following command in the beeline client prompt:
 
`set hive.execution.engine=mr;`

## Externalizing the Hive metastore to IBM Compose for MySQL

The Hive metastore is where the schemas of the Hive tables are stored. By default, it is in a MySQL instance in the cluster. You could choose to externalize the metastore to an external MySQL instance outside of the cluster so that you can tear down your cluster without losing the metadata. This, in combination with data in the object store, can preserve the data across clusters.

### Compose for MySQL
Compose for MySQL is a service in {{site.data.keyword.Bluemix_notm}} that can be used to externalize the metadata of the cluster. You can choose between the Standard or Enterprise version depending on your requirement. Once you create the Compose for MySQL instance, you will need to note the administrative user, password, database name, and the JDBC URL.

#### Compose for MySQL parameters to set in the Hive metastore

**DB_USER_NAME**: The database user name to connect to the instance, which is typically *admin*.

**DB_PWD**: The database user password to connect to the instance.

**DB_NAME**: The database name, which is typically *compose*.

**DB_CXN_URL**: The complete database connection URL. For example: ```jdbc:mysql://bluemix-sandbox-dal-9-portal.6.dblayer.com:12121?createDatabaseIfNotExist=true```.

**Note**: Make sure that you append ```“?createDatabaseIfNotExist=true”``` to the database connection URL or it might try to create tables again resulting in errors.

#### Configuring clusters to work with Compose for MySQL
There are two ways in which you can configure your cluster with the Compose for MySQL parameters:

* Using the Ambari user interface after the cluster has been created.
* Configuring the cluster as part of the cluster customization script.

##### Using the Ambari user interface after the cluster has been created

To configure the cluster:

1. Add the properties and values to the hive-site.xml file on your cluster instance by opening the Ambari console.
2. Open the advanced configuration for HIVE:

  `Ambari dashboard > Hive > Config > Advanced Tab > Hive Metastore > Existing MySQL / MariaDB Database`

3. Make the appropriate changes for the following parameters: Database Name, Database Username, Database Password, Database URL.
4. Save your changes.

**Important**: You will need to restart affected services as indicated in the Ambari user interface so that the changes take effect. This could take approximately three minutes.

**Note**: You might not be able to click **Test Connection** because of a known issue in the Ambari user interface.

##### Configuring the cluster as part of the cluster customization script
A customization script can be provided when the cluster is created. This script can provide the properties to be configured in the Hive site. It makes use of the Ambari configs.sh file to make the required changes.

The following is a sample script configuring the Hive metastore:
```
DB_USER_NAME=<admin>
DB_PWD=<SADFZCZVXZVC>
DB_NAME=<compose>
DB_CXN_URL=<jdbc:mysql://bluemix-sandbox-dal-9-portal.6.dblayer.com:12121?createDatabaseIfNotExist=true>

if [ "x$NODE_TYPE" == "xmanagement" ]
then

    echo "Node type is xmanagement hence updating ambari properties"
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST  $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionURL" $DB_CXN_URL /var/lib/ambari-server/resources/scripts/configs
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST  $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionUserName" $DB_USER_NAME
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST  $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionPassword" $DB_PWD
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME hive-site "ambari.hive.db.schema.name" $DB_NAME

    echo "stop and Start Services"
    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Stop All Services via REST"}, "ServiceInfo": {"state":"INSTALLED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 100

    curl -v --user $AMBARI_USER:$AMBARI_PASSWORD -H "X-Requested-By: ambari" -i -X PUT -d '{"RequestInfo": {"context": "Start All Services via REST"}, "ServiceInfo": {"state":"STARTED"}}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/services
    sleep 700
fi
```
{:codeblock}


For further information on Hive and its features, see [Apache Hive](https://hortonworks.com/apache/hive/).



