---

copyright:
  years: 2017, 2019
lastupdated: "2018-09-27"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Workaround for running Oozie jobs
{: #workaround-oozie}

When you run Oozie jobs, you might experience problems because of different versions of Oozie used in Hortonworks Data Platform (HDP).

Perform the steps in the following workaround to run:
 - [Spark jobs through Oozie](#run-spark-jobs-through-oozie)
 - [Pyspark jobs through Oozie](#run-pyspark-jobs-through-oozie)

## Run Spark jobs through Oozie
To run Spark jobs through Oozie:

1. Create a new Hadoop Distributed File System (HDFS)  directory, for example,
     `hadoop fs -mkdir /tmp/spark2`.

2. Copy the Spark jars from the  `/usr/hdp/current/spark2-client/jars/` directory to the newly created HDFS directory `/tmp/spark2`.

  **Note:** Do not copy the following JAR files which are also in `/usr/hdp/current/spark2-client/jars/`:
  - `aws-java-sdk-core-1.10.6.jar`
  - `aws-java-sdk-kms-1.10.6.jar`
  - `aws-java-sdk-s3-1.10.6.jar`
  - `azure-data-lake-store-sdk-2.1.4.jar`
  - `azure-keyvault-core-0.8.0.jar`
  - `azure-storage-5.3.0.jar`
  - `hadoop-aws-2.7.3.2.6.2.0-205.jar`
  - `hadoop-azure-2.7.3.2.6.2.0-205.jar`
  - `hadoop-azure-datalake-2.7.3.2.6.2.0-205.jar`
  - `okhttp-2.4.0.jar`
  - `okio-1.4.0.jar`

3. Copy the `oozie-examples.tar.gz`file to any directory and extract it. Then enter the following commands:

  a. `cp /usr/hdp/2.6.2.0-205/oozie/doc/oozie-examples.tar.gz $HOME`

  b. `gunzip oozie-examples.tar.gz`  	

  c. `tar -xf oozie-examples.tar`

4. Add the following properties `oozie.use.system.libpath` and `oozie.libpath` to the file `job.properties` which you got after exacting  `oozie-examples.tar.gz`. This example shows you what the file at `$HOME/examples/apps/spark/job.properties` contains:
  ```  
      nameNode=hdfs://<hostName>:8020
      # check the property fs.defaultFS in ambari/HDFS/Configs to get this actual value
	  jobTracker=<hostName>:8050
      # check the property yarn.resourcemanager.address in  ambari/YARN/Configs to get this actual value
	  master=yarn-cluster
	  queueName=default
	  examplesRoot=examples
	  oozie.use.system.libpath=true
	  oozie.wf.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/spark
	  oozie.use.system.libpath=true
	  oozie.libpath=/tmp/spark2
  ```

5. Edit the `workflow.xml` file and ensure that the Spark version is changed from `spark-action:0.1` to `spark-action:0.2`. This example shows you what the file at `$HOME/examples/apps/spark/workflow.xml` contains:
```    
   	 <workflow-app xmlns='uri:oozie:workflow:0.5' name='SparkFileCopy'>
    	<start to='spark-node' />
	    <action name='spark-node'>
        <spark xmlns="uri:oozie:spark-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <prepare>
                <delete path="${nameNode}/user/${wf:user()}/${examplesRoot}/output-data/spark"/>
            </prepare>
            <master>${master}</master>
            <name>Spark-FileCopy</name>
            <class>org.apache.oozie.example.SparkFileCopy</class>
            <jar>${nameNode}/user/${wf:user()}/${examplesRoot}/apps/spark/lib/oozie-examples.jar</jar>
            <arg>${nameNode}/user/${wf:user()}/${examplesRoot}/input-data/text/data.txt</arg>
            <arg>${nameNode}/user/${wf:user()}/${examplesRoot}/output-data/spark</arg>
        </spark>
        <ok to="end" />
        <error to="fail" />
    	</action>
        <kill name="fail">
        <message>Workflow failed, error
            message[${wf:errorMessage(wf:lastErrorNode())}]
        </message>
       </kill>
       <end name="end">
       </end>
       </workflow-app>```

6. Copy the files under `examples/apps/spark` and `examples/input-data` to the `/user/<userName>` folder structure in HDFS:
 ```
  hadoop fs -put examples /user/<userName>/examples```

7. Remove the following 3 jackson files and restart the Oozie service.
  	- ` /user/oozie/share/lib/lib_xxxxxxxx/oozie/jackson-annotations-2.4.0.jar`
	  - ` /user/oozie/share/lib/lib_xxxxxxxx/oozie/jackson-core-2.4.4.jar`
	  - ` /user/oozie/share/lib/lib_xxxxxxxx/oozie/jackson-databind-2.4.4.jar`

  If you don't remove the files, you will see the following message:
  ```
User class threw exception: java.lang.NoSuchMethodError:
com.fasterxml.jackson.databind.JavaType.isReferenceType()```

8. Run the Oozie job:
 ```  
 oozie job -oozie http://<hostName>:<portNumber>/oozie -config examples/apps/spark/job.properties -run
# check the property oozie.base.url in ambari/Oozie/Configs to get this actual value for hostname and portNumber)```

9. Open the Oozie Web UI (Ambari Console/Oozie/Oozie Web UI) and monitor the job.

10. If you need to further analyse why the job fails, you can get the application ID from the Oozie Web UI. The application ID looks something like `application_xxxxx77518806_0073`. To see the logs enter:
 ```
 yarn logs -applicationId application_xxxxx77518806_0073```

 Another way to find out why the job failed is to look at the error messages in the Oozie Web UI. For example, if you see the following error message:
```
Attempt to add (hdfs://<hostName>:8020/user/oozie/share/lib/lib_xxx/oozie/aws-java-sdk-kms-1.10.6.jar) multiple times to the distributed cache ```

 Then you might want to remove the  `aws-java-sdk-kms-1.10.6.jar` file from `/tmp/spark2`.

## Run PySpark jobs through Oozie

To run PySpark jobs through Oozie:

1. Create a `pi.py` file. Make sure there are no compilation issues with the command `python -m py compile pi.py`.

  ```
    import sys
    from random import random
    from operator import add
    from pyspark import SparkContext

    if __name__ == "__main__":
     """
     Usage: pi [partitions]
     """
    sc = SparkContext(appName="Python-Spark-Pi")
    partitions = int(sys.argv[1]) if len(sys.argv) > 1 else 2
    n = 100000 * partitions

    def f(_):
     x = random() * 2 - 1
     y = random() * 2 - 1
     return 1 if x ** 2 + y ** 2 < 1 else 0

    count = sc.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
    print("Pi is roughly %f" % (4.0 * count / n))
    sc.stop()
    ```
2. Create the `workflow.xml` file:
  ```
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
    </workflow-app>```

3. Create the `job.properties`file. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`:

  ```
  # Licensed to the Apache Software Foundation (ASF) under one
  # or more contributor license agreements.  See the NOTICE file
  # distributed with this work for additional information
  # regarding copyright ownership.  The ASF licenses this file
  # to you under the Apache License, Version 2.0 (the
  # "License"); you may not use this file except in compliance
  # with the License.  You may obtain a copy of the License at
  #
  #      http://www.apache.org/licenses/LICENSE-2.0
  #
  # Unless required by applicable law or agreed to in writing, software
  # distributed under the License is distributed on an "AS IS" BASIS,
  # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  # See the License for the specific language governing permissions and
  # limitations under the License.
  #

  nameNode=hdfs://chs-gwn-425-mn002.us-south.ae.appdomain.cloud.net:8020
  jobTracker=chs-gwn-425-mn002.us-south.ae.appdomain.cloud.net:8050
  master=yarn-cluster
  queueName=default
  examplesRoot=examples
  oozie.wf.application.path=${nameNode}/user/${user.name}/${examplesRoot}/apps/pyspark
  oozie.use.system.libpath=true
  oozie.libpath=/tmp/spark2
 ```

4. Create the following HDFS directories:
 - 	`/user/<userName>/examples/apps/spark/pyspark/`
 -	`/user/<userName>/examples/apps/spark/pyspark/lib`

5. Copy the `pi.py` file to `/user/<userName>/examples/apps/pyspark/lib`:
```
hadoop fs -copyFromLocal pi.py /user/<userName>/examples/apps/pyspark/lib ```

6. Copy the `workflow.xlm`file to  `/user/<userName>/examples/apps/pyspark/`
```
hadoop fs -copyFromLocal workflow.xml /user/<userName>/examples/apps/pyspark/workflow.xml```

7. Copy the python libraries to `/user/<userName>/examples/apps/pyspark/lib/py4j-0.10.4-src.zip` and `pyspark.zip`.
```
hadoop fs -copyFromLocal /usr/hdp/current/spark2-client/python/lib/p* /user/<userName>/examples/apps/pyspark/lib```

8. Run the Oozie job. The example uses the {{site.data.keyword.Bluemix_short}} hosting location `us-south`:
```
oozie job -oozie http://chs-gwn-425-mn002.us-south.ae.appdomain.cloud:11000/oozie -config pi/job.properties -run```

9. Open the Oozie Web UI (Ambari Console/Oozie/Oozie Web UI) and monitor the job.  If there are errors check the application ID and run the YARN command to check the logs.
