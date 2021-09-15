---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-30"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Enabling application autoscaling
{: #appl-auto-scaling}

It is not always possible to estimate the resource requirements (number of Spark executors) of a Spark application upfront, because these requirements might vary based on the size of the input data set.

To assist you in this situation, you can submit a Spark application with auto-scaling, which will automatically determine the number of executors required by an application based on the application's demand. You can enable basic autoscaling for a Spark application by adding the configuration setting  `ae.spark.autoscale.enable=true` to the existing application configuration.

## Submitting an application with basic autoscaling
{: #submit-autoscaling}

The steps to submit an application with autoscaling enabled is the same as the steps to submit an application without autoscaling. The only difference is that you need to add the configuration setting `ae.spark.autoscale.enable=true` to the application payload.

1. [Submit a Spark application](
https://test.cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#spark-submit-app).
1. Use the following sample JSON payload as an example to enable basic autoscaling:
    ```
    {
      "application_details": {
        "application": "/opt/ibm/spark/examples/src/main/python/wordcount.py",
        "arguments": ["/opt/ibm/spark/examples/src/main/resources/people.txt"]
       },
       "conf": {
         "ae.spark.autoscale.enable":"true"
       }
    }
    ```
    {: codeblock}

## Autoscaling application configurations

You can use the following configuration settings to further control autoscaling  the number of executors.   

| Configuration settings        | Description          | Default   |
|-----------------------|----------------------|---------- |
| `ae.spark.autoscale.enable` |	Signals that the application will autoscale based on the application's demand and any other autoscaling configurations that are set. If specified at instance level, all applications in the instance will autoscale.| false |
| `spark.dynamicAllocation.initialExecutors`| Specifies the initial number of executors to be created, irrespective of any demand made by the Spark application |	0 |
| `spark.dynamicAllocation.minExecutors` | Specifies the minimum number of executors to be maintained, irrespective of any demand by the Spark application | 0 |
| `spark.dynamicAllocation.maxExecutors` | Specifies the maximum number of executors to be created irrespective of any demand by the Spark Application | 2 |
| `ae.spark.autoscale.scaleupDemandPercentage` | Specifies the percentage of the executors demanded by Spark Application that should be fulfilled by the application auto-scaler. For example, if at a certain stage a Spark application requests 10 executors and the value for this configuration is set to 50 percent, the application auto-scaler will scale up the number of executors by 5 executors. The default value of this configuration is 100 percent which essentially means that any number of executors requested by an application's demand can be added by the application auto-scaler. | 100 |
| `ae.spark.autoscale.scaledownExcessPercentage` | Specifies the percentage of the idle executors held up by the Spark application that are to be removed by the application auto-scaler. For example, if after completing a certain stage in a Spark application, there are 20 idle executors held up by the application and the value for this configuration is set to 25 percent, the application auto-scaler will scale down the executors in the application by 4 executors. The default value for this configuration is 100 percent which means that the application auto-scaler can scale down all idle executors. | 100 |
| `ae.spark.autoscale.frequency `| The frequency in seconds at which the application auto-scaler scales up executors or scales down executors in an  application. | 	10s |


Example of an auto-scaling application payload showing the lower and upper bounds on the number of executors that the application can scale up or scale down to.

```
{
	"application_details": {
		"application": "cos://<application-bucket-name>.<cos-reference-name>/my_spark_application.py",
		"conf": {
			"ae.spark.autoscale.enable": "true",
			"spark.dynamicAllocation.initialExecutors": "0",
			"spark.dynamicAllocation.minExecutors": "1",
			"spark.dynamicAllocation.maxExecutors": "10",
			"spark.hadoop.fs.cos.<cos-reference-name>.endpoint": "s3.direct.us-south.cloud-object-storage.appdomain.cloud",
			"spark.hadoop.fs.cos.<cos-reference-name>.iam.api.key": "<iam-api-key-of-application-bucket>"
		}
	}
}
```
{: codeblock}

## Enabling autoscaling on an instance

By default, application autoscaling is disabled when you create an instance of the {{site.data.keyword.iae_short}} Standard serverless plan. If you want to enable automatic scaling for all applications at the instance level, you can explicitly set "ae.spark.autoscale.enable": "true" as a default Spark configuration when you create the instance. See [Default Spark configuration](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config).
