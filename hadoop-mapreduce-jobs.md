---

copyright:
  years: 2017
lastupdated: "2017-07-24"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Running Hadoop MapReduce jobs

**Pre-requisite**: Obtain the cluster user credentials, ssh and oozie_rest end point details from the service credentials of your service instance.

You can work with your data by analyzing the data with a Hadoop MapReduce program by opening the ssh connection to the cluster through a yarn command. For example:

```
yarn jar /usr/iop/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar  \
teragen  10000000   /user/clsadmin/teragen/test10G
```
{: codeblock}

If you are running MapReduce jobs with large workloads, consider enabling compression for the output to reduce the size of the intermediate data. To enable such compression, set the mapreduce.map.output.compress property to true in your command string. For example:

```
yarn jar /usr/iop/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort \
  -Dmapred.map.tasks=18 -Dmapred.reduce.tasks=36  \
  /user/clsadmin/teragen/test10G /user/clsadmin/terasort/test10Gsort
```
{: codeblock}

You can also run a MapReduce job by submitting the MapReduce job with Oozie through the oozie_rest endpoint URL. Complete the following steps to run a sample word count script.

**To run a sample word count script**

1. Create a MapReduce application directory in the HDFS and upload the application JAR file into the HDFS. SSH to the cluster as clsadmin and run the following commands:
```
[clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -mkdir /user/clsadmin/examples
[clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -mkdir /user/clsadmin/examples/apps
[clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -mkdir /user/clsadmin/examples/apps/mapreduce
[clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -mkdir /user/clsadmin/examples/apps/mapreduce/lib
[clsadmin@chs-xxxxx-mn003 ~]$ hdfs dfs -put /usr/iop/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
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
 For more information about workflow definitions, see the [Oozie Specification](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html), a Hadoop Workflow System.

3. Using the webhdfs REST API by using the curl command, upload workflow.xml into the Oozie application directory in the HDFS (/user/clsamdin/examples/apps/mapreduce).
```
curl -i -L  -s --user clsadmin:your_password --max-time 45 -X PUT -T workflow.xml \
https://XXXXX:8443/gateway/default/webhdfs/v1/\
user/clsadmin/examples/apps/mapreduce/workflow.xml?op=CREATE
```  

4. Using the webhdfs REST API, upload sample data into the HDFS. Create /user/clsadmin/examples/input-data/mapreduce and upload a sample file that is named sampledata.txt that contains your sample text.
```
curl -i -L  -s --user clsadmin:your_password --max-time 45 -X PUT -T sampledata.txt \
https://XXXXX:8443/gateway/default/webhdfs/v1/\
user/clsadmin/examples/input-data/mapreduce/sampledata.txt?op=CREATE
```

5. Create an Oozie job configuration file named oozie-mrjob-config.xml. Replace chs-XXXX-mn002 with the actual hostname of your cluster. For example:
```
<configuration>
 <property>
  <name>user.name</name>
  <value>clsadmin</value>
 </property>
 <property>
  <name>jobTracker</name>
  <value>chs-XXXX-mn002.bi.services.us-south.bluemix.net:8050</value>
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
  <value>hdfs://chs-XXXX-mn002.bi.services.us-south.bluemix.net:8020</value>
 </property>
</configuration>
```
 The value of xxxx is a unique number that is assigned to your cluster to identify the host name of your cluster's management node.

6. Start an Oozie job through the Oozie REST API by passing oozie-mrjob-config.xml (Oozie job configuration file) to the following curl command:
```
curl -i -s --user clsadmin:password -X POST \
-H "Content-Type: application/xml" -d@/path to oozie-mrjob-config.xml
https://XXXXX:8443/gateway/default/oozie/v1/jobs?action=start
```
 The command returns a JSON response that is similar to {"id":"0000005-150708104607239-oozie-oozi-W"}. Be sure to record the job ID value.

7. Check the job status. From the Ambari console, select **YARN** and click **Quick Links** &gt; **Resource Manager UI**. Select the job ID that matches the previous step result and view the job details.

8. Check the job output. After the job completes successfully, check the output by using the Hadoop file browser. View the results in /user/clsadmin/examples/output-data/map-reduce.
