---

copyright:
  years: 2017,2018
lastupdated: "2018-02-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Hive

The Apache Hive data warehousing software facilitates reading, writing, and managing large datasets that reside in distributed storage by using the SQL-like query language called HiveQL.

A compiler translates HiveQL statements into a directed acyclic graph of MapReduce or Tez jobs, which are submitted to Hadoop. In an {{site.data.keyword.iae_full_notm}} service, Hive commands can be executed through the Beeline client and by default, the Hive uses Tez as its execution engine. Note that Hive is not available in the {{site.data.keyword.iae_short}} Spark package.

## Prerequisites
To work with Hive, you need your cluster user credentials and the ssh and hive_jdbc end point details. You can get this information from the service credentials of your {{site.data.keyword.iae_short}} service instance.

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

## Accessing data in IBM CLoud Object Storage S3 from Hive  

Use the following example statement to access data in IBM Cloud Object Storage (COS) from Hive:
```
CREATE EXTERNAL TABLE myhivetable( no INT, name STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LOCATION 'cos://<bucketname>.<servicename>/dir1'; ```

`<bucketname>` is the COS bucket. The value for `<servicename>` can be any literal such as `iamservice` or `myprodservice`.


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
A customization script can be provided when the cluster is created. This script can provide the properties to be configured in the Hive site. It makes use of the Ambari configs.sh file to make the required changes. The script restarts only Hive, and tracks the progress instead of sleeping for a long random interval.

The following is a sample script configuring the Hive metastore:
```
#!/bin/bash
#-----------------------------------------------------------------------
# Customization script to point an IAE cluster's, Hive meta-store to an
# external mysql database. It is recommended to use Compose for MySQL
# as an external db. This scripts expects following four arguments:
# <db_user> <db_password> <db_name> <db_conn_url>
# Connection url shall be specified in the following format
# jdbc:mysql://<dbHost>:<dbPort>/<dbName>?createDatabaseIfNotExist=true
#-----------------------------------------------------------------------

# Helper functions

# Parse json and return value for the specified json path
parseJson ()
{
	jsonString=$1
	jsonPath=$2

	echo $(echo $jsonString | python -c "import json,sys; print json.load(sys.stdin)$jsonPath")
}

# Track progress using the call back returned by Ambari restart API
trackProgress ()
{
	response=$1
	# Extract call back to from response to track progress
	progressUrl=$(parseJson "$response" '["href"]')
	echo "Link to track progress: $progressUrl"

	# Progress tracking loop
	tempPercent=0
    while [ "$tempPercent" != "100.0" ]
	do
        progressResp=`curl -k -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By:ambari' -X GET $progressUrl --silent`
		tempPercent=$(parseJson "$progressResp" '["Requests"]["progress_percent"]')
		echo "Progress: $tempPercent"
		sleep 5s
	done

	# Validate if restart has really succeeded
	if [ "$tempPercent" == "100.0" ]
	then
		# Validate that the request is completed
		progressResp=`curl -k -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By:ambari' -X GET $progressUrl --silent`
		finalStatus=$(parseJson "$progressResp" '["Requests"]["request_status"]')
		if [ "$finalStatus" == "COMPLETED" ]
        then
        	echo 'Restart of affected service succeeded.'
            exit 0
        else
        	echo 'Restart of affected service failed'
            exit 1
        fi
	else
		echo 'Restart of affected service failed'
		exit 1
	fi
}

# Validate input
if [ $# -ne 4 ]
then
	 echo "Usage: $0 <db_user> <db_password> <db_name> <db_conn_url>"
else
	DB_USER_NAME="$1"
	DB_PWD="$2"
	DB_NAME="$3"
	DB_CXN_URL="$4"
fi


# Actual customization starts here
if [ "x$NODE_TYPE" == "xmanagement-slave2" ]
then    

	echo "Updating Ambari properties"
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionURL" $DB_CXN_URL
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionUserName" $DB_USER_NAME
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME hive-site "javax.jdo.option.ConnectionPassword" $DB_PWD
    /var/lib/ambari-server/resources/scripts/configs.sh -u $AMBARI_USER -p $AMBARI_PASSWORD -port $AMBARI_PORT -s set $AMBARI_HOST $CLUSTER_NAME hive-site "ambari.hive.db.schema.name" $DB_NAME

    echo 'Restart services/components affected by Hive configuration change'
    response=`curl -k -u $AMBARI_USER:$AMBARI_PASSWORD -H 'X-Requested-By: ambari' --silent -w "%{http_code}" -X POST -d '{"RequestInfo":{"command":"RESTART","context":"Restart all required services","operation_level":"host_component"},"Requests/resource_filters":[{"hosts_predicate":"HostRoles/stale_configs=true"}]}' https://$AMBARI_HOST:$AMBARI_PORT/api/v1/clusters/$CLUSTER_NAME/requests`

    httpResp=${response:(-3)}
    if [[ "$httpResp" != "202" ]]
    then
		echo "Error initiating restart for the affected services, API response: $httpResp"
		exit 1
    else
		echo "Request accepted. Hive restart in progress...${response::-3}"
		trackProgress "${response::-3}"
    fi
fi  
```
{:codeblock}

For the complete source code to the customization script to point an {{site.data.keyword.iae_full_notm}} cluster's Hive meta-store to an external MySQL database, see [here]( https://github.com/IBM-Cloud/IBM-Analytics-Engine/blob/master/customization-examples/associate-external-metastore.sh).

## Learn more

For further information on Hive and its features, see [Apache Hive](https://hortonworks.com/apache/hive/).
