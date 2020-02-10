---

copyright:
  years: 2017, 2019
lastupdated: "2018-06-19"

subcollection: AnalyticsEngine

---

# Working with Sqoop
{: #working-with-sqoop}

Apache Sqoop is a tool for efficiently transferring bulk data between Apache Hadoop and structured data stores, such as relational databases.

## Using Sqoop to connect to DB2 or MySQL

To work with Sqoop, you need your cluster user credentials and the SSH  and the JDBC endpoint details.

1. SSH to the cluster.
  - The `db2jcc4.jar` file is in  `/home/common/lib/dataconnectorDb2`.
  ```
      ls -ltr /home/common/lib/dataconnectorDb2
      -rwxrwxrwx. 1 root root 3411523 May 23 13:27 /home/common/lib/dataconnectorDb2/db2jcc4.jar
  ```
  -   The sqljdbc4.jar file is in `/home/common/lib/R/DatabaseConnector/java`.
  ```
     ls -ltr /home/common/lib/R/DatabaseConnector/java
     total 7484
     -rwxrwxrwx. 1 root root  584207 May 23 14:12 sqljdbc4.jar
     -rwxrwxrwx. 1 root root 2447255 May 23 14:12 RedshiftJDBC4-1.1.17.1017.jar
     -rwxrwxrwx. 1 root root  588974 May 23 14:12 postgresql-9.3-1101.jdbc4.jar
     -rwxrwxrwx. 1 root root 2739616 May 23 14:12 ojdbc6.jar
     -rwxrwxrwx. 1 root root  954041 May 23 14:12 mysql-connector-java-5.1.30-bin.jar
     -rwxrwxrwx. 1 root root  317816 May 23 14:12 jtds-1.3.1.jar
  ```

2. Create the directory `/sqoop/lib` under `/home/wce/clsadmin`.

3. Copy the files `/home/common/lib/dataconnectorDb2/db2jcc4.jar` and `/home/common/lib/R/DatabaseConnector/java/sqljdbc4.jar` to `/home/wce/clsadmin/sqoop/lib`. Make sure that the permission of the JAR files and its parent directories is 755.
```
    cp /home/common/lib/dataconnectorDb2/db2jcc4.jar /home/wce/clsadmin/sqoop/lib

    cp /home/common/lib/R/DatabaseConnector/java/sqljdbc4.jar /home/wce/clsadmin/sqoop/lib
```
4. On the Ambari interface, select the Sqoop service. Then, click **Configs > Advanced**, and expand the `sqoop-env` section. Add the  variables named **DB2_JARS** and **MSSQL_JARS**, and append them to SQOOP_USER_CLASSPATH as follows:
```
    export DB2_JARS=/home/wce/clsadmin/sqoop/lib/db2jcc4.jar
    export MSSQL_JARS=/home/wce/clsadmin/sqoop/lib/sqljdbc4.jar

    export SQOOP_USER_CLASSPATH="`ls ${HIVE_HOME}/lib/libthrift-*.jar 2>/dev/null`:${DB2_JARS}:${MSSQL_JARS}${SQOOP_USER_CLASSPATH}"
```
5. Restart Sqoop and run Service Check.
6. Verify that you can load the driver and connect to MySQL for example by entering:
```
sqoop eval --connect jdbc:mysql://<changeMeDatabaseHost>:<changeMeDatabasePort>/<changeMeDatabaseName> --username <changeMeDatabaseUser> --password <changeMeDatabasePassword> --query "show tables"
```   
7. Examples of some commands:

 - To import from a table to HDFS and COS, enter:
 ```
sqoop eval --connect jdbc:mysql://<changeMeDatabaseHost>:<changeMeDatabasePort>/<changeMeDatabaseName> --username <changeMeDatabaseUser> --password <changeMeDatabasePassword> --query "select * from VERSION"
 ```
 - To import a table VERSION to HDFS, enter:
 ```
sqoop import --connect jdbc:mysql://<changeMeDatabaseHost>:<changeMeDatabasePort>/<changeMeDatabaseName> --username <changeMeDatabaseUser> --password <changeMeDatabasePassword>  --table VERSION -m 1 --target-dir /user/VERSION
 ```
 - To import a table VERSION to Cloud Object Store bucket, enter:
 ```
 sqoop import --connect jdbc:mysql://<changeMeDatabaseHost>:<changeMeDatabasePort>/<changeMeDatabaseName> --username <changeMeDatabaseUser> --password <changeMeDatabasePassword>  --table VERSION -m 1 --target-dir cos://<changeMeCOSbucket>.myprodservice/VERSION
 ```
 - To connect to Db2, enter:
 ```
     cat jdbcprops
     retrieveMessagesFromServerOnGetMessage=true
     sslConnection=true
     sslTrustStoreLocation=/home/wce/clsadmin/truststore.jks
     sslTrustStorePassword=changeit

    [clsadmin@chs-fkk-299-mn003 ~]$ sqoop list-tables --connect jdbc:db2:// <changeMeDatabaseHost>/<changeMeDatabaseName> --username <changeMeDatabaseUserName> --password <changeMeDatabasePassword> --connection-param-file jdbcprops
  ```

  **Note:** You can use the same steps to import data from other databases by adding the respective JAR files to the Sqoop user classpath.  
