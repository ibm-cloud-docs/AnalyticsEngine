---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-14"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Configuring Spark log level information
{: #config_log}

Review the applications that run and identify the issues that are present by using the logs that the {{site.data.keyword.iae_full_notm}} Spark application generates. The standard logging levels available are ALL, TRACE, DEBUG, INFO, WARN, ERROR, FATAL, and OFF. By default, the {{site.data.keyword.iae_short}} Spark application logs at the WARN log level for spark drivers and OFF for spark executors. You can configure the logging level to display relevant, and fewer verbose messages.
{: shortdesc}

The {{site.data.keyword.iae_full_notm}} logging configuration configures the log level information of the Spark framework. It does not affect the logs written by the user code using commands such as 'logger.info()', 'logger.warn()', 'print()', or'show()' in the Spark application.
{: important}

## Configuring options
{: #log-optn}

Configure the following {{site.data.keyword.iae_full_notm}} logs to set up the Spark application log level:
* Spark driver logs (by using `ae.spark.driver.log.level`)
* Spark executor logs (by using `ae.spark.executor.log.level`)

Specify the option in the Spark configurations section at the time of provisioning an {{site.data.keyword.iae_full_notm}} instance or submitting a Spark application.
You can specify the following standard log level values:
* ALL
* TRACE
* DEBUG
* INFO
* WARN
* ERROR
* FATAL
* OFF


The default value for driver log level is `WARN` and executor log level is `OFF`.
{: note}

You can apply the configuration in the following two ways:
* Instance level configuration
* Application level configuration

### Configuring Spark log level information at the instance level
{: #inst_level}

At the time of provisioning an {{site.data.keyword.iae_full_notm}} instance, specify the log level configurations under the default_config attribute. For more information, see [Default Spark configuration](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config).


Example :

```bash

    "default_config": {
        "ae.spark.driver.log.level": "WARN",
        "ae.spark.executor.log.level": "ERROR"
    }
```
{: codeblock}


### Configuring Spark log level information at the application level
{: #smpl-job}

At the time of submitting a job, specify the options in the payload under `conf`. For more information, see [Spark application REST API](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api).

Example :

```bash

{
     "conf": {
	"ae.spark.driver.log.level":"WARN",
	"ae.spark.executor.log.level":"WARN",
     }
}
```
{: codeblock}


## Sample use case
{: #smpl-usecase}

**Setting the log-level Spark configuration at an instance level** : The sample use case considers the scenario where you provision an {{site.data.keyword.iae_full_notm}} instance and configure the log level such that all the applications in the instance log at ERROR.

1. Set the following configurations as default Spark configurations:

     * ae.spark.driver.log.level = ERROR
     * ae.spark.executor.log.level = ERROR

     After setting the default Spark configuration, the log level for all applications that are submitted to the instance is set to `ERROR` (provided the application payload does not specify the Spark configuration during submission).

**Setting log-level Spark configuration at job level** : The sample use case considers a scenario where you have an application and the log level configuired such that logs are logged at the INFO level. You can specify the spark configuration in the payload. Consider the sample payload:

```bash
{
  "application_details": {
     "application": "cos://<application-bucket-name>.<cos-reference-name>/my_spark_application.py",
     "arguments": ["arg1", "arg2"],
     "conf": {
        "spark.hadoop.fs.cos.<cos-reference-name>.endpoint": "https://s3.direct.us-south.cloud-object-storage.appdomain.cloud",
        "spark.hadoop.fs.cos.<cos-reference-name>.access.key": "<access_key>",
        "spark.hadoop.fs.cos.<cos-reference-name>.secret.key": "<secret_key>",
        "spark.app.name": "MySparkApp",
     "ae.spark.driver.log.level":"INFO",
     "ae.spark.executor.log.level":"INFO",
     }
  }
}
```
{: codeblock}

In the [sample use case](#smpl-usecase), the Spark application overrides the log level Spark configuration set at instance level that is, ERROR to INFO.
{: note}
