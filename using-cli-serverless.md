---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-29"

keywords: analytics engine CLI, end-to-end scenario, tutorial for analytics engine cli

subcollection: analyticsengine

content-type: tutorial
completion-time: 15m 

---

{{site.data.keyword.attribute-definition-list}}


# Explore creating serverless instances and submitting applications using the CLI
{: #using-cli}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

Learn how to use the {{site.data.keyword.iae_full_notm}} CLI to create the services that you need to create and manage a serverless instance, and submit and monitor your Spark applications.
{: shortdesc}

You create a serverless instance by selecting the {{site.data.keyword.iae_full_notm}} Standard serverless plan. When a serverless instance is provisioned, an Apache Spark cluster is created, which you can customize with library packages of your choice, and is where you run your Spark applications.


## Objectives
{: #iae-objectives}

You will learn how to install and set up the following services and components that you will need to use the CLI:

- An {{site.data.keyword.cos_full_notm}} instance in which your {{site.data.keyword.iae_full_notm}} instance stores custom application libraries and Spark history events.
- A {{site.data.keyword.cos_short}} bucket for application files and data files.
- An {{site.data.keyword.iae_full_notm}} serverless instance. This instance is allocated compute and memory resources on demand whenever Spark workloads are deployed. When an application is not in running state, no computing resources are allocated to the instance. The price is based on the actual usage of resources consumed by the instance, billed on a per second basis.
- Logging service to help you to troubleshoot issues that might occur in the {{site.data.keyword.iae_full_notm}} instance and submitted application, as well as to view any output generated by your application. When you run applications with logging enabled, logs are forwarded to an {{site.data.keyword.la_full_notm}} service where they are indexed, enabling full-text search through all generated messages and convenient querying based on specific fields.

## Before you begin
{: #ae-cli-prereqs}

To start using the the {{site.data.keyword.iae_short}} V3 CLI you need:

- An {{site.data.keyword.Bluemix_short}} account.
- To install the {{site.data.keyword.cloud_notm}} CLI. See [Getting started with the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started) for the instructions to download and install the CLI.

Now you can start using the {{site.data.keyword.iae_short}} V3 CLI. You must follow the instructions in steps 1 and 2 to install the required services before you start with step 3 to upload and submit Spark applications. Step 4 shows you how to create a logging instance and enable logging. Step 5 shows you how to delete an Analytics Engine instance, although this step is optional. 

## Create a Cloud Object Storage instance and retrieve credentials
{: #create-cos-get-creds}
{: step}

Create an IBM Cloud Object Storage instance and retrieve the Cloud Object Storage credentials (service keys) by using the Analytics Engine Serverless CLI.
{: shortdesc}

The Cloud Object Storage instance that you create is required by the {{site.data.keyword.iae_full_notm}} instance as its *instance home* storage for Spark history events and any custom libraries or packages that you want to use in your applications. See [Instance home](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#instance-home). 

For more information on how you can create a library set with custom packages that is stored in Cloud Object Storage and referenced from your application, see [Using a library set](/docs/AnalyticsEngine?topic=AnalyticsEngine-use-lib-set).

1. Log in to {{site.data.keyword.Bluemix_short}} using your {{site.data.keyword.Bluemix_short}} account.

    Action
    :   Enter:
        ```sh
        ibmcloud api <URL>
        ibmcloud login
        ```
        {: codeblock}

    Example
    :   Enter:
        ```sh
        ibmcloud api https://cloud.ibm.com
        ibmcloud login
        ```
        {: codeblock}

1. Select the resource group. Get the list of the resource groups for your account and select one in which to create the IBM Analytics Engine serverless instance:

    Action
    :   Enter:
        ```sh
        ibmcloud target -g RESOURCE_GROUP_NAME
        ```
        {: codeblock}

    Parameters: 
    - RESOURCE_GROUP_NAME: The name of resource group in which the serverless instance is to reside
        

    Example
    :   Enter:
        ```sh
        ibmcloud resource groups
        ibmcloud target -g default
        ```
        {: codeblock}

1. Install the IBM Cloud Object Storage service and then the Analytics Engine V3 CLI:

    Action
    :   Enter:
        ```sh
        ibmcloud plugin install cloud-object-storage
        ```
        {: codeblock}

    Action
    :   Enter:
        ```sh
        ibmcloud plugin install analytics-engine-v3
        ```
        {: codeblock}

1. Create a Cloud Object Storage instance:

    Action
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create INSTANCE_NAME cloud-object-storage PLAN global
        ```
        {: codeblock}

    Parameters: 
    - INSTANCE_NAME: Any name of your choice
    - PLAN: The Cloud Object Storage plan to use when creating the instance

    Example
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create test-cos-object cloud-object-storage standard global
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```text
        Service instance test-cos-object was created. 
        Name:             test-cos-object 
        ID:               crn:v1:bluemix:public:cloud-object-storage:global:a/867d444f64594fd68c7ebf4baf8f6c90:ebad3176-8a1a-41f2-a803-217621bf6309:: 
        GUID:             ebad3176-8a1a-41f2-a803-217621bf6309 
        Location:         global 
        State:            active 
        Type:             service_instance 
        Sub Type: 
        Allow Cleanup:    false 
        Locked:           false 
        Created at:       2021-12-27T07:57:56Z 
        Updated at:       2021-12-27T07:57:58Z 
        Last Operation: 
                         Status    create succeeded 
                         Message   Completed create instance operation
       ```
1. Configure the CRN by copying the value of ID from the response the of Cloud Object Storage creation call in the previous step:

    Action
    :   Enter:
        ```sh
        ibmcloud cos config crn
        Resource Instance ID CRN: ID
        ```
        {: codeblock}

    Parameters: 
    - ID: The value of ID from the response the of Cloud Object Storage creation call


    Example
    :   Enter:
        ```sh
        ibmcloud cos config crn 
        Resource Instance ID CRN: crn:v1:bluemix:public:cloud-object-storage:global:a/867d444f64594fd68c7ebf4baf8f6c90:ebad3176-8a1a-41f2-a803-217621bf6309::
        ```
        {: codeblock}

1. Create a Cloud Object Storage bucket:

    Action
    :   Enter:
        ```sh
        ibmcloud cos bucket-create --bucket BUCKET_NAME [--class CLASS_NAME] [--ibm-service-instance-id ID] [--region REGION] [--output FORMAT]
        ```
        {: codeblock}

        Parameters: 
        - BUCKET_NAME: Any name of your choice
        - ID: The value of GUID from the response the of Cloud Object Storage creation call
        - REGION: The IBM Cloud region in which the Cloud Object Storage instance was created 
        - FORMAT: Output format can be JSON or text.

    Example
    :   Enter:
        ```sh
        ibmcloud cos bucket-create --bucket test-cos-storage-bucket --region us-south --ibm-service-instance-id ebad3176-8a1a-41f2-a803-217621bf6309 --output json
        ```
        {: codeblock}

1. Create Cloud Object Storage service keys:

    Action
    :   Enter:
        ```sh
        ibmcloud resource service-key-create NAME [ROLE_NAME] ( --instance-id SERVICE_INSTANCE_ID | --instance-name SERVICE_INSTANCE_NAME | --alias-id SERVICE_ALIAS_ID | --alias-name SERVICE_ALIAS_NAME) [--service-id SERVICE_ID] [-p, --parameters @JSON_FILE|JSON_TEXT] [-g RESOURCE_GROUP] [--service-endpoint SERVICE_ENDPOINT_TYPE] [--output FORMAT] [-f, --force] [-q, --quiet] 
        ```
        {: codeblock}

        Parameters: 
        - NAME: Any name of your choice
        - [ROLE_NAME]: This parameter is optional. The access role, for example, `Writer` or `Reader` 
        - SERVICE_INSTANCE_ID: The value of GUID from the response the of Cloud Object Storage creation call
        - SERVICE_INSTANCE_NAME: The value of NAME from the response the of Cloud Object Storage creation call
        - JSON_TEXT: The authentication to access Cloud Object Storage. Currently only HMAC keys are supported.
       
    Example
    :   Enter:
        ```sh
        ibmcloud resource service-key-create test-service-key-cos-bucket Writer --instance-name test-cos-object --parameters '{"HMAC":true}'
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```text
        Creating service key of service instance test-cos-object under account Test  
        OK 

        Service key crn:v1:bluemix:public:cloud-object-storage:global:a/183**93b485e:9ee135f9-4667-4797-8478-b20**ce-key:21a310e1-bbd6-**bf1f4 was created. 
        Name:          test-service-key-cos-bucket 
        ID:            crn:v1:bluemix:public:cloud-object-** 
        Created At:    Mon Dec 27 12:52:49 UTC 2021 
        State:         active 
        Credentials: 
            apikey: 3a4Ncm**o-WJGFaEzwfY 
            cos_hmac_keys: 
               access_key_id: 21a31**f1f4 
               secret_access_key: c5a23**b6792d3e0a6c 
               endpoints: https://control.cloud-object-storage.cloud.ibm.com/v2/endpoints 
               iam_apikey_description: Auto-generated for key crn:v1:bluemix:public:cloud-object-storage:global:a/1836f778**c93b485e:9ee**8478-b2019a4b4e20:resource-key:21a3**05a9bf1f4 
               iam_apikey_name: test-service-key-cos-bucket 
               iam_role_crn: crn:v1:bluemix:public:iam::::serviceRole:Writer 
               iam_serviceid_crn: crn:v1:bluemix:public:iam-identity::a/1836f77885e521c5ab2523aac93b485e::serviceid:ServiceId-702ca222-3615-464c-92d3-1849c03170cc 
               resource_instance_id: crn:v1:bluemix:public:cloud-object-storage:global:a/1836f7**3b485e:9ee135f9-4667-479**4e20:: 
        ```

## Create an Analytics Engine serverless instance
{: #create-iae-instance-cli}
{: step}

Create an serverless Analytics Engine instance by using the CLI.
{: shortdesc}

1. Create the Analytics Engine service instance:

    Action
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create INSTANCE_NAME ibmanalyticsengine standard-serverless-spark us-south -p @provision.json 
        ```
        {: codeblock}

    Parameters:
    - INSTANCE_NAME: Any name of your choice
    - @provision.json: Structure the JSON file as shown in the following example. Use the access and secret key from the response the of Cloud Object Storage service key creation call.

    Example of the provision.json file
    :   Sample JSON file:
        ```json
        {
           "default_runtime": {
              "spark_version": "3.1" }, 
              "instance_home": {
                 "region": "us-south", 
                 "endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud", 
                 "hmac_access_key": "<your-hmac-access-key>", 
                 "hmac_secret_key": "<your-hmac-secret-key>"} 
        } 
        ```
        {: codeblock}

    Example
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create test-ae-service ibmanalyticsengine standard-serverless-spark us-south -p @ae_provision.json 
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```text
        Creating service instance test-ae-service in resource group <Resource Group Name> of account <Account Name> as <email id>... 

        OK 

        Service instance test-ae-service was created. 
        Name:                test-ae-service 
        ID:                  crn:v1:bluemix:public:ibmanalyticsengine:us-south:a/183**aac93b485e:181ea**be1-70978**1b:: 
        GUID:                181ea**9ee01b 
        Location:            us-south 
        State:               provisioning 
        Type:                service_instance 
        Sub Type: 
        Service Endpoints:   public 
        Allow Cleanup:       false 
        Locked:              false 
        Created at:          2022-01-03T08:40:25Z 
        Updated at:          2022-01-03T08:40:26Z 
        Last Operation: 
            Status    create in progress 
            Message   Started create instance operation 
        ```
    
1. Check the status of the Analytics Engine service:

    Action
    :   Enter:
        ```sh
        ibmcloud ae-v3 instance show –id INSTANCE_ID 
        ```
        {: codeblock}

    Parameters:
    - INSTANCE_ID: The value of GUID from the response the of Analytics Engine instance creation call

    Example
    :   Enter:
        ```sh
        ibmcloud ae-v3 instance show –id 181ea**9ee01b 
        ```
        {: codeblock}
     
    Response
    :   The example returns:
        ```json
        {
           "default_runtime": {
              "spark_version": "3.1" }, 
           "id": "181ea**9ee01b ", 
           "instance_home": { 
              "bucket": "do-not-delete-ae-bucket-e96**5d-b7**a82", 
              "endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud", 
              "hmac_access_key": "**", 
              "hmac_secret_key": "**", 
              "provider": "ibm-cos", 
              "region": "us-south", 
              "type": "objectstore" }, 
           "state": "active", 
           "state_change_time": "**" 
        }  
        ```
     
    Only submit your Spark application when the state of the Analytics Engine service is active.
    {: note}

## Upload and submit a Spark application
{: #uploadandsubmit}
{: step}

Upload an application file to Cloud Object Storage and submit a Spark application. 
{: shortdesc}

This tutorial shows you how to add the Spark application to the Cloud Object Storage instance bucket that is used as *instance home* by the Analytics Engine instance. If you want to separate the instance related files from the files you use to run your applications, for example the applications files themselves, data files, and any results of your analysis, you can use a different bucket in the same Cloud Object Storage instance or use a different Cloud Object Storage instance.

1. Upload the Spark application file:

    Action
    :   Enter:
        ```sh
        ibmcloud cos upload --bucket BUCKET_NAME --key KEY --file PATH [--concurrency VALUE] [--max-upload-parts PARTS] [--part-size SIZE] [--leave-parts-on-errors] [--cache-control CACHING_DIRECTIVES] [--content-disposition DIRECTIVES] [--content-encoding CONTENT_ENCODING] [--content-language LANGUAGE] [--content-length SIZE] [--content-md5 MD5] [--content-type MIME] [--metadata STRUCTURE] [--region REGION] [--output FORMAT] [--json] 
        ```
        {: codeblock}

    Parameters:

    - BUCKET_NAME: Name of bucket used when bucket was created
    - KEY: Application file name
    - PATH: file name and path to the Spark application file


    Example
    :   Enter:
        ```sh
        ibmcloud cos upload --bucket test-cos-storage-bucket --key test-math.py --file test-math.py 
        ```
        {: codeblock}

    Sample application file
    :   Sample of `test-math.py`:
        ```python
        from pyspark.sql import SparkSession
        import time
        import random
        import cmath
       ​
        def init_spark():
          spark = SparkSession.builder.appName("test-math").getOrCreate()
          sc = spark.sparkContext
          return spark,sc

        def transformFunc(x):
          return cmath.sqrt(x)+cmath.log(x)+cmath.log10(x)
          ​
        def main():
          spark,sc = init_spark()
          partitions=[10,5,8,4,9,7,6,3]
          for i in range (0,8):
            data=range(1,20000000)
            v0 = sc.parallelize(data, partitions[i])
            v1 = v0.map(transformFunc)
            print(f"v1.count is {v1.count()}. Done")
            time.sleep(60)
            
        if __name__ == '__main__':
          main()
        ```
        {: codeblock}

1. Check the status of the Analytics Engine service:

    Action
    :   Enter:
        ```sh
        ibmcloud ae-v3 instance show –id INSTANCE ID 
        ```
        {: codeblock}

    Parameters:
    - INSTANCE_ID: The value of GUID from the response the of Analytics Engine instance creation call

    Example
    :   Enter:
        ```sh
        ibmcloud ae-v3 instance show –id 181ea**9ee01b 
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```json
        {
           "default_runtime": {
              "spark_version": "3.1" }, 
           "id": "181ea**9ee01b ", 
           "instance_home": { 
              "bucket": "do-not-delete-ae-bucket-e96**5d-b7**a82", 
              "endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud", 
              "hmac_access_key": "**", 
              "hmac_secret_key": "**", 
              "provider": "ibm-cos", 
              "region": "us-south", 
              "type": "objectstore" }, 
           "state": "active", 
           "state_change_time": "**" 
        }  
        ```
     
    Only submit your Spark application when the state of the Analytics Engine service is active.
    {: note}

1. Submit the Spark application:

    Action
    :   Enter:
        ```sh
        ibmcloud ae-v3 spark-app submit --instance-id INSTANCE_ID -–app APPLICATION_PATH
        ```
        {: codeblock}

    Parameters: 
    - INSTANCE_ID: The value of GUID from the response the of Analytics Engine instance creation call
    - APPLICATION_PATH: The file name and path to the Spark application file

    Example
    :   Enter:
        ```sh
        ibmcloud ae-v3 spark-app submit --instance-id 181ea**9ee01b --app "cos://test-cos-storage-bucket.mycos/test-math.py" --conf '{"spark.hadoop.fs.cos.mycos.endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud", "spark.hadoop.fs.cos.mycos.access.key": "21**bf1f4", "spark.hadoop.fs.cos.mycos.secret.key": "c5a**d3e0a6c"}'  
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```text
        id      7f7096d2-5c44-4d9a-ac01-b904c7611b7b 
        state   accepted 
        ```
     
1. Check the details or status of the application that you submitted:

    Action
    :   Enter:
        ```sh
        ibmcloud ae-v3 spark-app show --instance-id INSTANCE_ID --app-id APPLICATION_ID 
        ```
        {: codeblock}

    Parameters: 
    - INSTANCE_ID: The value of GUID from the response the of Analytics Engine creation call
    - APPLICATION_ID: The value of id from the response of the spark-app submit call

    Example
    :   Enter:
        ```sh
        ibmcloud ae-v3 spark-app show --instance-id 181ea**9ee01b --app-id 7f7096d2-5c44-4d9a-ac01-b904c7611b7b 
        ```
        {: codeblock}

    Response
    :   The example returns:
        ```text
        application_details   <Nested Object> 
        id                    7f7096d2-5c44-4d9a-ac01-b904c7611b7b 
        state                 finished 
        start_time            2022-03-01T12:58:54.000Z 
        finish_time           2022-03-01T13:09:14.000Z 
        ```

## Create logging service to see logs
{: #logging-with-cli}
{: step}

You can use use the Analytics Engine CLI to enable logging to help you troubleshoot issues in IBM Analytics Engine. Before you can enable logging, you need to create an IBM Log Analysis service instance to which the logs are forwarded.
{: shortdesc}

1. Create a logging instance:

    Action
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create NAME logdna SERVICE_PLAN_NAME LOCATION 
        ```
        {: codeblock}

    Parameters: 
    - NAME: Any name of your choice for the IBM Log Analysis service instance
    - SERVICE_PLAN_NAME: The name of the service plan. For valid values, see [Service plans](/docs/log-analysis?topic=log-analysis-service_plans).
    - LOCATION: Locations where Analytics Engine is enabled to send logs to IBM Log Analysis. For valid locations, see [Compute serverless services](/docs/log-analysis?topic=log-analysis-cloud_services_locations#cloud_services_locations_serverless).

    Example
    :   Enter:
        ```sh
        ibmcloud resource service-instance-create my-log-instance logdna 7-day us-south
        ```
        {: codeblock}

    Once the logging service is created, you can log in to [{{site.data.keyword.Bluemix_short}}](cloud.ibm.com), search for the logging service instance, and click on the monitoring dashboard. There you can view the driver and executor logs, as well as all application logs for your Spark application.  
    
    Search using the `application_id` or `instance_id`.
    {: tip}
    
1. Enable logging:

    Action
    :   Enter:
        ```sh
        ibmcloud analytics-engine-v3 log-config COMMAND [arguments...] [command options] 
        ```
        {: codeblock}

    Parameters: 
    - `analytics-engine-v3`: Use `ae-v3` to use the v3 CLI commands
    - COMMAND: Use the `update` command to enable logging
    

    Example
    :   Enter:
        ```sh
        ibmcloud ae-v3 log-config update --instance-id 181ea**9ee01b --enable --output json 
        ```
        {: codeblock}

## Delete Analytics Engine instance
{: #delete-iae-instance-cli}
{: step}

You can use the CLI to delete an instance, for example if you need an instance with a completely different configuration to handle greater workloads.
{: shortdesc}

You can retain an Analytics Engine instance as long as you want and submit your Spark applications against the same instance on an as-needed basis.

If you want to delete an Analytics Engine instance: 

Action
:   Enter:
    ```sh
    ibmcloud resource service-instance-delete NAME|ID [-g RESOURCE_GROUP] -f 
    ```
    {: codeblock}

Parameters: 
- NAME|ID: The value of Name or GUID from the response of the Analytics Engine instance creation call
- RESOURCE_GROUP: Optional parameter. The name of resource group in which the serverless instance is resides


Example
:   Enter:
    ```sh
    ibmcloud resource service-instance-delete MyServiceInstance  -g default  -f
    ```
    {: codeblock}
    

## Learn more
{: #learn-more-cli}

See the [IBM Analytics Engine serverless CLI reference](/docs/AnalyticsEngine?topic=analytics-engine-cli-plugin-CLI_analytics_engine).