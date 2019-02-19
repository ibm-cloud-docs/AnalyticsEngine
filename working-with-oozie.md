---

copyright:
  years: 2017, 2019
lastupdated: "2018-09-26"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with Oozie
{: #working-with-oozie}

Oozie is a workflow scheduler system that you can use to manage Hadoop jobs. Oozie is integrated with the rest of the Hadoop stack supporting  Hadoop MapReduce jobs.

## Submitting MapReduce jobs with Oozie

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
 For more information about workflow definitions, see the [Oozie Specification](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html), a Hadoop workflow system.

3. Upload workflow.xml into the Oozie application directory in the HDFS (/user/clsadmin/examples/apps/mapreduce):
```
hdfs dfs -put workflow.xml /user/clsadmin/examples/apps/mapreduce/
```

4. Upload sample data into the HDFS. Create /user/clsadmin/examples/input-data/mapreduce and upload a sample file that is named sampledata.txt and contains your sample text:
```
hdfs dfs -mkdir -p /user/clsadmin/examples/input-data/mapreduce
hdfs dfs -put sampledata.txt /user/clsadmin/examples/input-data/mapreduce
```

5. Create an Oozie job configuration file named oozie-mrjob-config.xml. Replace chs-XXXX-mn002 with the actual hostname of your cluster and  `<changeme>` with the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`, `eu-gb` (for the United Kingdom), `eu-de` (for Germany) or `jp-tok` (for Japan).
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
 The value of xxxx is a unique identifier of your cluster. You can get this identifier by inspecting your cluster’s service credentials.

6. Submit an Oozie job through the Oozie REST API by passing oozie-mrjob-config.xml (Oozie job configuration file) in the following curl command:
```
curl -i -s --user clsadmin:password -X POST \
-H "Content-Type: application/xml" -d@/path to oozie-mrjob-config.xml
https://XXXXX:8443/gateway/default/oozie/v1/jobs?action=start
```
 The command returns a JSON response that is similar to {"id":"0000005-150708104607239-oozie-oozi-W"}. Be sure to record the job ID value.

7. Check the job status. From the Ambari console, select **YARN** and click **Quick Links** &gt; **Resource Manager UI**. Select the job ID that matches the previous step result and view the job details.

8. Check the job output. After the job completes successfully, check the output by using the Hadoop file browser. View the results in /user/clsadmin/examples/output-data/map-reduce.
