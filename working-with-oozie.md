---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-22"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Oozie
{: #working-with-oozie}

Oozie is a workflow scheduler system that you can use to manage Hadoop jobs. Oozie is integrated with the rest of the Hadoop stack from where you can invoke Spark, Hive, and MapReduce jobs.

## Invoking Oozie workflows

You can invoke Oozie jobs in two ways:

- By using the cURL REST API to invoke jobs from outside the cluster
- By using the Oozie CLI to invoke jobs from the cluster after you SSH to the cluster

The following code samples show invoking Oozie workflows or jobs through Oozie with input or output data from {{site.data.keyword.cos_short}} using both methods.

## Submitting MapReduce jobs through Oozie

You can run a MapReduce job by submitting the MapReduce job with Oozie through the oozie_rest endpoint URL.

To run a sample word count job with Oozie:

1. Create a MapReduce application directory in the HDFS and upload the application JAR file into the HDFS. SSH to the cluster as cluster user and run the following commands:

    ```
    [clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -mkdir -p /user/clsadmin/examples/apps/mapreduce/lib
    [clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -put /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    /user/clsadmin/examples/apps/mapreduce/lib
    ```
2. Create a workflow definition (workflow.xml) that runs a MapReduce job with Oozie. For example:

    ```
    <workflow-app xmlns="uri:oozie:workflow:0.5" name="map-reduce-wf">
     <start to="mr-node"/>
      <action name="mr-node">
       <map-reduce>
        <job-tracker>${jobTracker}</job-tracker>
        <name-node>${nameNode}</name-node>
        <configuration>
         <property>
          <name>mapred.mapper.new-api</name>
          <value>true</value>
         </property>
         <property>
          <name>mapred.reducer.new-api</name>
          <value>true</value>
         </property>
         <property>
          <name>mapred.job.queue.name</name>
          <value>${queueName}</value>
         </property>
         <property>
          <name>mapreduce.map.class</name>
          <value>org.apache.hadoop.examples.WordCount$TokenizerMapper</value>
         </property>
         <property>
          <name>mapreduce.reduce.class</name>
          <value>org.apache.hadoop.examples.WordCount$IntSumReducer</value>
         </property>
         <property>
          <name>mapreduce.combine.class</name>
          <value>org.apache.hadoop.examples.WordCount$IntSumReducer</value>
         </property>
         <property>
          <name>mapred.output.key.class</name>
          <value>org.apache.hadoop.io.Text</value>
         </property>
         <property>
          <name>mapred.output.value.class</name>
          <value>org.apache.hadoop.io.IntWritable</value>
         </property>
         <property>
          <name>mapred.input.dir</name>
          <value>/user/clsadmin/examples/input-data/mapreduce</value>
         </property>
         <property>
          <name>mapred.output.dir</name>
          <value>/user/clsadmin/examples/output-data/mapreduce</value>
         </property>
        </configuration>
       </map-reduce>
       <ok to="end"/>
       <error to="fail"/>
      </action>
      <kill name="fail">
       <message>Map/Reduce failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
      </kill>
     <end name="end"/>
    </workflow-app>
    ```

      For more information about workflow definitions, see the [Oozie Specification](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html).

3. Upload `workflow.xml` to the Oozie application directory in  HDFS (`/user/clsadmin/examples/apps/mapreduce`):

    ```
    hdfs dfs -put workflow.xml /user/clsadmin/examples/apps/mapreduce/
    ```
4. Upload sample data to HDFS. Create the directory `/user/clsadmin/examples/input-data/mapreduce` and upload your sample data in the file `sampledata.txt`:

    ```
    hdfs dfs -mkdir -p /user/clsadmin/examples/input-data/mapreduce
    hdfs dfs -put sampledata.txt /user/clsadmin/examples/input-data/mapreduce
    ```
5. Create an Oozie job configuration file named `oozie-mrjob-config.xml`. Replace `chs-XXXX-mn002` with the actual hostname of your cluster and  `<changeme>` with the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).

    ```
    <configuration>
     <property>
      <name>user.name</name>
      <value>clsadmin</value>
     </property>
     <property>
      <name>jobTracker</name>
      <value>chs-XXXX-mn002.<changeme>.ae.appdomain.cloud:8050</value>
     </property>
     <property>
      <name>oozie.wf.application.path</name>
      <value>/user/clsadmin/examples/apps/mapreduce</value>
     </property>
     <property>
      <name>queueName</name>
      <value>default</value>
     </property>
     <property>
      <name>nameNode</name>
      <value>hdfs://chs-XXXX-mn002.<changeme>.ae.appdomain.cloud:8020</value>
     </property>
    </configuration>
    ```

    The value `xxxx` is the unique identifier of your cluster. You can get this identifier by checking your cluster’s service credentials.

6. Submit an Oozie job through the Oozie REST API by passing `oozie-mrjob-config.xml` (Oozie job configuration file) in the following cURL command:

    ```
    curl -i -s --user clsadmin:password -X POST \
    -H "Content-Type: application/xml" -d@/path to oozie-mrjob-config.xml
    https://XXXXX:8443/gateway/default/oozie/v1/jobs?action=start
    ```

    The command returns a JSON response that is similar to `{"id":"0000005-150708104607239-oozie-oozi-W"}`. Save the job ID value.
7. Check the job status. From the Ambari console, select **YARN** and click **Quick Links** &gt; **Resource Manager UI**. Select the job ID that matches the previous step result and view the job details.
8. Check the job output in the Hadoop file browser. View the results in `/user/clsadmin/examples/output-data/map-reduce`.

## Submitting Spark jobs through Oozie

You can run a Spark job through Oozie.

The following code samples show you how to copy a file from one directory to another in a simple Spark application.

1. Copy the `oozie-examples.tar.gz` file to any directory and extract it.
2. Enter the following commands:

    ```
    cp /usr/hdp/<latest_version>/oozie/doc/oozie-examples.tar.gz $HOME
    gunzip oozie-examples.tar.gz
    tar -xf oozie-examples.tar
    ```
3. Edit the `$HOME/examples/apps/spark/job.properties` file to reflect the correct values for `nameNode` and `jobTracker`. See an example of the job properties file:

    ```
    nameNode=hdfs://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8020
    # check the property fs.defaultFS in ambari/HDFS/Configs to get this actual value
    jobTracker=chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8050
    # check the property yarn.resourcemanager.address in  ambari/YARN/Configs to get this actual value
    master=yarn-cluster
    queueName=default
    examplesRoot=examples
    oozie.use.system.libpath=true
    oozie.wf.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/spark
    ```
4. Copy the examples folder into HDFS:

    ```
    hadoop fs -put examples /user/<userName>/examples
    ```
5. Run the Oozie job:

    ```
    oozie job -oozie http://<hostName>:<portNumber>/oozie -config examples/apps/spark/job.properties -run
    ```

    You can get the values for the variables `<hostName>` and `<portNumber>` from the property `oozie.base.url` by opening the Ambari console and then, on the dashboard, clicking **Oozie >  Configs**.

    Note that this example shows you how to run the Oozie job from the cluster. The earlier example that used Mapreduce showed how to run the Oozie job using the cURL command.
6. Test that the output file was created correctly:

    ```
    hadoop fs -ls /user/<userName>/examples/output-data/spark/
    ```

## Submitting Hive jobs through Oozie

You can run Hive jobs through Oozie. The following example shows you how to create a Hive external table using data from the input directory, read the table content, and output the content to an  output directory.

1. Edit the `job.properties` with the appropriate values for  `nameNode`, `jobTracker` and `jdbcURL`. See an example of the job properties file:

    ```
    vi $HOME/examples/apps/hive2/job.properties
    nameNode=hdfs://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8020
    jobTracker=chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8050
    queueName=default
    jdbcURL= jdbc:hive2://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2
    #check the property HIVESERVER2 JDBC in ambari/HIVE/SUMMARY to get this actual value
    examplesRoot=examples
    oozie.use.system.libpath=true
    oozie.wf.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/hive2
    ```
1. Run the Oozie job:

    ```
    oozie job -oozie http://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:11000/oozie -config examples/apps/hive2/job.properties -run
    ```
1. Check the results:

    ```
    hadoop fs -cat /user/clsadmin/examples/output-data/hive2/000000_0
    ```

    If you don’t see any results, check that the input directory exists and that the file isn't empty.

1. Test your changes on a file in {{site.data.keyword.cos_full_notm}}. To do this, you must first  edit the `$HOME/examples/apps/hive2/workflow.xml` file and change the input or output directory to point to an  {{site.data.keyword.cos_full_notm}} location. For example, to `cos://mybucket.s3service/output-data/spark`.

## Running PySpark jobs through Oozie

You can run PySpark jobs through Oozie.

1. Create a local directory for PySpark jobs and copy the application `pi.py` file to this directory:

    ```
    mkdir examples/apps/pyspark
    mkdir examples/apps/pyspark/lib
    cp /usr/hdp/current/spark2-client/examples/src/main/python/pi.py examples/apps/pyspark/lib/
    ```
1. Add a `workflow.xml` file with the following content to the `pyspark` directory:

    ```
    vi examples/apps/pyspark/workflow.xml  
    <workflow-app xmlns='uri:oozie:workflow:0.5' name='SparkPythonPi'>
       <start to='spark-node' />
       <action name='spark-node'>
         <spark xmlns="uri:oozie:spark-action:0.2">
           <job-tracker>${jobTracker}</job-tracker>
           <name-node>${nameNode}</name-node>
           <master>${master}</master>
           <name>Python-Spark-Pi</name>
           <jar>pi.py</jar>
         </spark>
         <ok to="end" />
         <error to="fail" />
       </action>

       <kill name="fail">
         <message>Workflow failed, error message [${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name='end' />
    </workflow-app>
    ```
1. Add a `job.properties` file with the following content to the `pyspark` directory:

    ```
    nameNode=hdfs://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8020
    jobTracker=chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8050
    master=yarn-cluster
    queueName=default
    examplesRoot=examples
    oozie.wf.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/pyspark
    	 oozie.libpath=/user/oozie/share/lib/lib_20190708174627/spark/
    #Make sure that you have the correct timestamp from the actual directory on HDFS
    oozie.use.system.libpath=true
    ```
1. Copy the `pyspark` directory to HDFS:

    ```
    hadoop fs -put examples/apps/pyspark/ /user/clsadmin/examples/apps
    ```
1. Execute the Oozie job:

    ```
    oozie job -oozie http://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:11000/oozie -config examples/apps/pyspark/job.properties -run
    ```
1. Check the application logs for the value of Pi:

    ```
    yarn logs -applicationId application_1562609820633_0032 | grep "Pi is roughly"  
    ```

    You should find: Pi is roughly 3.141020.

## Running jobs off data in Object Storage

You can run each of the Oozie workflows or jobs described in the previous sections with different input or output data stored in {{site.data.keyword.cos_short}}.     

All you need to do is to update the workflow to point to the different {{site.data.keyword.cos_short}} data file locations. For example,  you can update `$HOME/examples/apps/spark/workflow.xml` and change the input or output directory to another {{site.data.keyword.cos_short}} location, such as `cos://mybucket.s3service/output-data/spark`.

## Running Oozie coordinator jobs

Coordinator jobs are used to schedule workflow jobs. In other words they work like a cron scheduler.

The following example shows you how to schedule to run the previous PySpark Pi application using a coordinator job that runs every 2 hours.  

1. Begin by creating a `coordinator.xml` file in the `pyspark` directory for the pyspark workflow file you created earlier. The frequency is set to every two hours.

    ```
    vi /home/wce/clsadmin/examples/apps/pyspark/coordinator.xml

    <coordinator-app xmlns="uri:oozie:coordinator:0.2" name="coord_pyspark" frequency="${coord:hours(2)}" start="${start}" end="${end}" timezone="${tz}">
        <action>
         <workflow>
            <app-path>/user/clsadmin/examples/apps/pyspark/workflow.xml</app-path>
         </workflow>
        </action>
    </coordinator-app>  
    ```
1. Create a new `job1.properties` file in the same path:

    ```
    nameNode=hdfs://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8020
    jobTracker=chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:8050
    master=yarn-cluster
    queueName=default
    examplesRoot=examples
    workflowAppUri=${nameNode}/user/${user.name}/${examplesRoot}/apps/pyspark
    oozie.coord.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/pyspark
    oozie.libpath=/user/oozie/share/lib/lib_20190708174627/spark/
    oozie.use.system.libpath=true
    start=2019-07-11T00:00Z
    end=2019-12-31T01:00Z
    tz=IST
    ```

    Note that the previous properties include the location of the coordinator application, the start and end times, and the time zone.
1. Upload the `coordinator.xml` file to HDFS:

    ```
    hadoop fs -put /home/wce/clsadmin/examples/apps/pyspark/coordinator.xml
    /user/clsadmin/examples/apps/pyspark/
    ```
1. Run the Oozie application:

    ```
    oozie job -oozie http://chs-qwu-777-mn002.eu-gb.ae.appdomain.cloud:11000/oozie -config examples/apps/pyspark/job1.properties -run
    ```
1. Verify that the application runs as expected from the Oozie and Yarn UIs in the Ambari console.
