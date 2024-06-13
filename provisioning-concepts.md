---

copyright:
  years: 2017, 2023
lastupdated: "2023-10-10"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Architecture and concepts in serverless instances
{: #serverless-architecture-concepts}

This topic shows you the architecture of {{site.data.keyword.iae_full_notm}} serverless instances and describes some key concepts and definitions.

## Instance architecture
{: #serverless-architecture}

The {{site.data.keyword.iae_full_notm}} service is managed by using {{site.data.keyword.Bluemix_short}} Identity and Access Management (IAM). As an {{site.data.keyword.Bluemix_notm}} account owner, you are assigned the account administrator role.

With an {{site.data.keyword.Bluemix_notm}} account, you can provision and manage your serverless {{site.data.keyword.iae_short}} instance by using the:
- {{site.data.keyword.Bluemix_notm}} console
- CLI
- REST API

The {{site.data.keyword.iae_short}} microservices in the control plane, accessed through an API gateway handle instance creation, capacity provisioning, customization and runtime management while your Spark applications run in isolated namespaces in the data plane. Each Spark application that you submit runs in its own Spark cluster, which is a combination of Spark master and executor nodes. See [Isolation and network access](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-model-serverless#isolation-network-access).

Each {{site.data.keyword.iae_short}} instance is associated with an {{site.data.keyword.cos_full_notm}} instance for instance related data that is accessible by all applications that run in the instance. Currently, all Spark events are stored in this instance as well. Spark application logs are aggregated to a {{site.data.keyword.la_short}} log server.

![Shows the {{site.data.keyword.iae_full_notm}} serverless instance architecture.](images/AE-serverless-architecture.svg){: caption="Figure 1. Architecture flow diagram of IBM Analytics Engine" caption-side="bottom"}


## Key concepts
{: #key-concepts}

With {{site.data.keyword.iae_full_notm}} serverless instances, you can spin up Apache Spark clusters as needed and customize the Spark runtime and default Spark configuration options.

The following sections describe key concepts when provisioning serverless instances.

### {{site.data.keyword.iae_full_notm}} service instance
{: #service-instance}

An {{site.data.keyword.Bluemix_short}} service is cloud extension that provides ready-for-use functionality, such as database, messaging, and web software for running code, or application management or monitoring capabilities. Services usually do not require installation or maintenance and can be combined to create applications. An instance of a service is an entity that consists of resources that are reserved for a particular application or a service.

When you create an {{site.data.keyword.iae_full_notm}} from the catalog, you will give the service instance a name of your choice, select the default Spark runtime you want to associate with the instance and provide the default Spark configuration to use with the instance. Additionally, you need have to specify the `Instance home`, which is the storage attached to the instance for instance related data only.

**Note**:

- When you create an {{site.data.keyword.iae_full_notm}} service instance, no costs are incurred unless you have Spark applications running or the Spark history server is accessed.
- Costs are incurred if {{site.data.keyword.cos_full_notm}} if accessed through public endpoints, and when you enable forwarding {{site.data.keyword.iae_full_notm}} logs to {{site.data.keyword.la_full_notm}}.
- There is a default limit on the number of service instances permitted per {{site.data.keyword.Bluemix_short}} account and on the amount of CPU and memory that can be used in any given {{site.data.keyword.iae_full_notm}} service instance. See [Limits and quotas for {{site.data.keyword.iae_short}} instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits). If you need to adjust these limits, open an IBM Support ticket.
- There is no limit on the number of Spark applications that can be run in an {{site.data.keyword.iae_full_notm}} service instance.


### Default Spark runtime
{: #default-spark-runtime}

At the time of instance provisioning, you can select the Spark version to be used. Currently, you can choose between Spark 3.3 and Spark 3.4. Spark 3.3 is considered as the default version.

The runtime includes open source Spark binaries and the configuration helps you to quickly proceed with the instance creation and run Spark applications in the instance. In addition to the Spark binaries, the runtime also includes the geospatial, data skipping, and Parquet modular encryption libraries.

Across all Spark runtime version, you can submit Spark applications written in the following languages:
* Scala
* Python
* R

The following table shows the Spark runtime version and runtime language version.

| Spark version | Apache Spark release	 | status| Supported Languages |
|-----------|--------------|---------|---- |
| 3.1 | 3.1.2 | Removed(Not Supported) | Java 8, Scala 2.12, Python 3.10 and R 4.2 |
| 3.3 | 3.3.2 | Default | Java 11, Scala 2.12, Python 3.10 and R 4.2 |
| 3.4 | 3.4.1 | Latest | Java 11, Scala 2.12, Python 3.10 and R 4.2 |
{: caption="Table 1. Spark runtime version and runtime language version" caption-side="top"}
{: #features-table-1}
{: row-headers}

The language versions are upgraded periodically to keep the runtime free from any security vulnerabilities.
You can always override the Spark runtime version when you submit an application. For details on what to add to the payload, see [Passing the runtime Spark version when submitting an application](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#pass-spark-version).
{: important}



### Instance home
{: #instance-home}

Instance home is the storage attached to the instance for instance related data only, such as custom application libraries and Spark history events. Currently, only {{site.data.keyword.cos_full_notm}} is accepted for instance home. This instance can be an instance in your {{site.data.keyword.Bluemix_short}} account or an instance from a different account.

When you provision an instance using the {{site.data.keyword.Bluemix_notm}} console, the {{site.data.keyword.cos_full_notm}} instances in your {{site.data.keyword.Bluemix_short}} account are auto discovered and displayed in a list for you to select from. If no {{site.data.keyword.cos_full_notm}} instances are found in your account, you can use the REST APIs to update instance home after instance creation.

You can't change instance home after instance creation. You can only edit the access keys.



### Default Spark configuration
{: #default-spark-config}


You can specify default Spark configurations at the time of provisioning an {{site.data.keyword.iae_short}} instance (See [Provisioning an {{site.data.keyword.iae_full_notm}} serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-serverless)). The configurations are automatically applied to the Spark applications submitted on the instance. You can also update the configurations after creating the instance. You can edit the configuration from the **Configuration** section in the **{{site.data.keyword.iae_short}} Instance details** page, [{{site.data.keyword.iae_short}} Rest APIs](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3#replace-instance-default-configs) or [IAE CLI](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-CLI_analytics_engine#analytics-engine-v3-cli-instance-default-configs-command) . Values specified as instance level defaults can be overridden at the time of submitting Spark applications.

To learn more about the various Apache Spark configurations, see [Spark Configuration](https://spark.apache.org/docs/latest/configuration.html).



## Serverless instance features and execution methods
{: #default-spark-config-1}

The following table shows the supported serverless instance features by access role and execution methods.

| Operation | Access roles | IBM Console | API | CLI |
|-----------|--------------|---------|---- |-----|
| Provision instances | Administrator | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Delete instances | Administrator | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Grant users permission | Administrator | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Manage instance home storage | Administrator | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Configure logging | Administrator   \n  Developer   \n  Devops | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | Not available |
| Submit Spark applications | Administrator   \n  Developer | Not available | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| View list of submitted Spark applications | Administrator   \n  Developer   \n  DevOps | Not available | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Stop submitted Spark applications | Administrator   \n  Developer   \n  DevOps | Not available | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Customize libraries | Administrator   \n  Developer | Not available | ![the confirm icon](images/confirm.png) | Not available |
| Access job logs | Administrator    \n  Developer   \n  DevOps | ![the confirm icon](images/confirm.png) from the {{site.data.keyword.la_short}} console | Not applicable | Not applicable |
| View instance details; shown details might vary depending on access role | Administrator   \n  Developer   \n  DevOps | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Manage Spark history server | Administrator   \n  Developer   | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
| Access Spark history | Administrator   \n  Developer   \n  DevOps | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
{: caption="Table 2 Supported serverless instance features by access role and execution methods" caption-side="top"}
{: #features-table-1}
{: row-headers}
