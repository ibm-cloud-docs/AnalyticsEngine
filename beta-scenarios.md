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

# Beta scenarios: Using Spark SQL and running a Spark application
IBM Analytics Engine provides a flexible framework to develop and deploy analytics applications in Apache Hadoop and Apache Spark. It allows you to create and manage clusters using the {{site.data.keyword.Bluemix_notm}} interface or using the Cloud Foundry CLI and REST APIs.

Two scenarios provide tasks that show you how to query data using the Spark SQL through a Data Science Experience (DSX) notebook and how to run a Spark application using Spark submit. An open data set from the City of New York containing calls to the 311 number to report issues with infrastructure will be used as sample data for the scenarios.

## Uploading data
Before you start executing the scenario, you need to bring in the sample data. IBM Analytics Engine is based on Apache Hadoop and Spark. While it provides the HDFS file system and a limited amount of storage in the cluster, we recommend using IBM Cloud Object Store or the Swift-based {{site.data.keyword.objectstoragefull}} service to store data.

Jobs from an Analytics Engine compute cluster can be run against data in object stores, and results of jobs can be written back to the object store.
The sample data set is available [here](https://github.com/wdp-beta/get-started).

To upload data into the object store, refer to documentation of the respective offerings: [IBM Cloud Object Store](https://ibm-public-cos.github.io/crs-docs/) / [{{site.data.keyword.objectstoragefull}}](https://console.bluemix.net/docs/services/ObjectStorage/index.html).

## Scenarios

### Querying data using Spark SQL through a DSX notebook
**Spark SQL** is a **Spark** module for structured data processing. Unlike the basic **Spark** RDD API, the interfaces provided by Spark SQL provide **Spark** with more information about the structure of both the data and the computation being performed. You can interact with **Spark** SQL using SQL and the Dataset API.

**To connect your DSX instance to an IBM Analytics Engine cluster**

1. Log in to [DSX](https://datascience.ibm.com/). Ensure that your DSX account is associated with the same {{site.data.keyword.Bluemix_notm}} account, organization, and space that you created the IBM Analytics Engine cluster with.

2. Create a new project in DSX or open an existing DSX project that you want to use with IBM Analytics Engine.

3. Select the project's **Settings** tab and scroll down to see the **Associated Services** list.

4. Click **add associated service**. A menu of services is displayed.

5. Select **IBM Analytics Engine**.

6. On the next screen, select your IBM Analytics Engine instance that you created in {{site.data.keyword.Bluemix_notm}}.

Now this project has been associated with the instance of IBM Analytics Engine that you previously created. You are ready to start running queries and jobs from a DSX notebook using Apache Spark in IBM Analytics Engine.

A Jupyter notebook [here](https://github.com/wdp-beta/get-started) includes steps and instructions get you started with analyzing data by using SparkSQL. Add the notebook to your project in DSX and run it.


### Running a simple Spark application using Spark submit
You can run Spark applications locally or distributed across a cluster, either by using an interactive shell or by submitting an application. In this task, you will learn how to submit a batch job to count words in a text file on HDFS.

**To submit a batch job**

1. Upload a text file to HDFS in your cluster. You can do this by using either the Files View in Ambari or WebHDFS APIs. The instructions [here](https://console.bluemix.net/docs/services/AnalyticsEngine/Upload-files-to-HDFS.html#uploading-files-to-hdfs) will help you upload files to HDFS.

2. SSH into the cluster.

3. Copy the script wordcount.py provided [here](https://github.com/wdp-beta/get-started)  to /home/wce/clsadmin/.

4. Go to **Manage Cluster** in {{site.data.keyword.Bluemix_notm}} and click the **nodes** tab to get the name node host name. It's the host name of the **management-slave1** node type.

4. Go to dir /usr/iop/current/spark2-client/bin.

5. Submit the script using the spark-submit command:<br>
```spark-submit --master yarn --deploy-mode client --executor-memory 1g --name wordcount --conf "spark.app.id=wordcount" /home/wce/clsadmin/wordcount.py hdfs://<name_node_host_name>:8020/input_file_path 2```

After the program runs, the output will be at /home/wce/clsadmin/output.txt in your local directory of the management node that you SSH into.

For more information on submitting Spark jobs, refer to [spark-submit](https://console.bluemix.net/docs/services/AnalyticsEngine/wce-cli-ref-spark-submit.html#spark-submit) in the documentation.
