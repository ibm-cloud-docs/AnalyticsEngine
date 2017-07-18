---

copyright:
  years: 2017
lastupdated: "2017-07-17"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Run Hadoop MapReduce jobs

You can work with your data by analyzing the data with a Hadoop MapReduce program. If you are running MapReduce jobs with large workloads, consider enabling compression for the output to reduce the size of the intermediate data. To enable such compression, set the mapreduce.map.output.compress property to true in your command string. For example:

```
yarn jar /usr/iop/4.2.0.0/hadoop-mapreduce/hadoop-mapreduce-examples.jar terasort
  -Dmapred.map.tasks=18 -Dmapred.reduce.tasks=36
  -Dmapreduce.map.output.compress=true
  /user/ceuser/teragen/test3Ttera /user/ceuser/teragen/test3Ttsort
 ```
 {:codeblock}
 
You can run a MapReduce job by submitting the MapReduce job with Oozie through the Oozie REST API. Complete the following steps to run a sample word count script.

**To run a sample word count script**
1. Create a MapReduce application directory in the HDFS and upload the application JAR file into the HDFS. From the shell node, SSH to the Ambari server node as *your_username* and run the following commands:

```
[@shl01 ~]$ hdfs dfs -mkdir /user/your_username/examples
[@shl01 ~]$ hdfs dfs -mkdir /user/your_username/examples/apps
[@shl01 ~]$ hdfs dfs -mkdir /user/your_username/examples/apps/mapreduce
[@shl01 ~]$ hdfs dfs -mkdir /user/your_username/examples/apps/mapreduce/lib
[@shl01 ~]$ hdfs dfs -put /usr/iop/4.2.0.0/hadoop-mapreduce/hadoop-mapreduce-examples.jar
 /user/your_username/examples/apps/mapreduce/lib
 ```
 {:codeblock}
 
2. Create a workflow definition (workflow.xml) that runs a MapReduce job with Oozie. Replace *your_username* with your cluster user name. For example:

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
      <value>/user/your_username/examples/input-data/mapreduce</value>
     </property>
     <property>
      <name>mapred.output.dir</name>
      <value>/user/your_username/examples/output-data/mapreduce</value>
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
{:codeblock}

For more information about workflow definitions, see the [Oozie Specification](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html), a Hadoop Workflow System.

3. Using the Ambari file browser, upload workflow.xml into the Oozie application directory in the HDFS (/user/your_username/examples/apps/mapreduce).

4. Using the Ambari file browser, upload sample data into the HDFS. Create /user/your_username/examples/input-data/mapreduce and upload a sample file that is named sampledata.txt that contains your sample text.

5. Create an Oozie job configuration file named oozie-mrjob-config.xml. For example:

```
<configuration>
 <property>
  <name>user.name</name>
  <value>your_username</value>
 </property>
 <property>
  <name>jobTracker</name>
  <value>bi-hadoop-prod-xxxx.services.dal.bluemix.net:8050</value>
 </property>
 <property>
  <name>oozie.wf.application.path</name>
  <value>/user/your_username/examples/apps/mapreduce</value>
 </property>
 <property>
  <name>queueName</name>
  <value>default</value>
 </property>
 <property>
  <name>nameNode</name>
  <value>bi-hadoop-prod-xxxx.services.dal.bluemix.net:8020</value>
 </property>
</configuration>
```
{:codeblock]

The value of xxxx is a unique number that is assigned to your cluster to identify the host name of your cluster's management node.

6. Start an Oozie job through the Oozie REST API by passing the Oozie job configuration file to the following curl command:

```
curl -i -s --user your_username:password -X POST -H "Content-Type: application/xml" -d @oozie-mrjob-config.xml
https://bi-hadoop-prod-xxxx.services.dal.bluemix.net:8443/gateway/default/oozie/v1/jobs?action=start
```
{:codeblock}

The command returns a JSON response that is similar to {"id":"0000005-150708104607239-oozie-oozi-W"}. Be sure to record the job ID value.

7. Check the job status. From the Ambari console, select **YARN** and click **Quick Links** &gt; **Resource Manager UI**. Select the job ID that matches the previous step result and view the job details.Check the job output. After the job completes successfully, check the output by using the Hadoop file browser. View the results in /user/your_username/examples/output-data/map-reduce.
 
