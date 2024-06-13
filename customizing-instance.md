---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-31"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Customization overview
{: #cust-instance}

You can customize a serverless instance specifically to suit your application needs, over and above what is provisioned on a default basis.

For example, you might want to install custom analytics third-party libraries or you might want to fine-tune some cluster configurations, for example, the Spark default configurations.

You can customize an instance at any point of its lifecycle. The customizations are applied only to those applications that are submitted after you added the customization. They are not applied to an currently running applications.

## Customization options
{: #cust-options}

You can customize your instance by:
-	Specifying configuration values that are inherited by all Spark applications that run in the instance
- Making Python, R, Scala or custom libraries available to your Spark applications

When you create an instance you can:

- Specify default values for configuration properties and environment variables supported by the Apache Spark configuration. You can specify configuration properties and environment variables as name-value pairs that are saved at the instance level and passed to all Spark applications that run in the instance. These default configuration parameters can simplify the payload that is passed when submitting a Spark application. You can also override these values at the time a Spark application is submitted.

    For a list of the default Spark configurations and environment variables, see [Spark configurations](https://spark.apache.org/docs/latest/configuration.html).
- Customize the instance with libraries required by your Spark applications after instance creation. You can create a library set that packages all libraries to be made available to all Spark applications that run in the instance, and then refer to this defined library set at the time the Spark application is submitted.

    To create a library set, see [Creating a library set](/docs/AnalyticsEngine?topic=AnalyticsEngine-create-lib-set).

**Note**:

- The maximum size limit of a customization library set is 2 GB.
- The start time of your Spark application or the time taken for additional executors to get added in an autoscaling scenario, is proportional to the size of the custom library set. Therefore it is a best practice to limit a library set to only the files that are  needed for a specific application. If other applications require different sets of files, it is better to use different library sets.
