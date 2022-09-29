---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-29"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
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

![Shows the {{site.data.keyword.iae_full_notm}} serverless instance architecture.](images/AE-serverless-architecture.svg)

<!--
You are billed only for the compute resources consumed when your applications run. For more details on billing, see [{{site.data.keyword.iae_full_notm}}   Pricing](https://www.ibm.com/cloud/analytics-engine/pricing){: external}.
{: note}-->

## Key concepts
{: #key-concepts}

With {{site.data.keyword.iae_full_notm}} serverless instances, you can spin up Apache Spark clusters as needed and customize the Spark runtime and default Spark configuration options.

The following sections describe key concepts when provisioning serverless instances.

### Default Spark runtime
{: #default-spark-runtime}

You can select which Spark version to use when the instance is provisioned. Currently, you can choose between Spark 3.1 and Spark 3.3. If you don't select a Spark runtime version,  Spark 3.1 is taken by default. 

The runtime contains only open source Spark binaries and is configured to help you to quickly get started to create and run Spark applications in the instance. In addition to the Spark binaries, the runtime also includes the geospatial, data skipping, and Parquet modular encryption libraries.

On a Spark 3.1 or Spark 3.3 runtime, you can submit Spark applications written in the following languages: Scala 2.12, Python 3.9, and R 3.6.3.

Note that the language versions are upgraded periodically to keep the runtime free from any security vulnerabilities.

### Instance home
{: #instance-home}

Instance home is the storage attached to the instance for instance related data only, such as custom application libraries and Spark history events. Currently, only {{site.data.keyword.cos_full_notm}} is accepted for instance home. This instance can be an instance in your {{site.data.keyword.Bluemix_short}} account or an instance from a different account.

When you provision an instance using the {{site.data.keyword.Bluemix_notm}} console, the {{site.data.keyword.cos_full_notm}} instances in your {{site.data.keyword.Bluemix_short}} account are auto discovered and displayed in a list for you to select from. If no {{site.data.keyword.cos_full_notm}} instances are found in your account, you can use the REST APIs to update instance home after instance creation.

You can’t change instance home after instance creation. You can only edit the access keys.

<!--
### Resource quota
{: #resource-quota}

A resource quota provides constraints at the service instance level on the amount of resources that can be used at any point in time. By enforcing a quota at the time the instance is created, you can control the hardware resources, such as CPU and memory, that are used by all applications and kernels at any point in time.

You must provide a quota for:

- CPU cores: The total number of virtual CPUs across the instance. The default is 80.
- Memory in GiB: The total amount of memory in gibibytes that can be requested by applications or kernels across the instance. The default is 100 GiB. -->

### Default Spark configuration
{: #default-spark-config}

You can specify default Spark configurations at the instance level and let that be inherited by Spark applications created on the instance. This is an optional section that you can specify at the time of instance creation. Values specified as instance level defaults can be overridden at the time of submitting Spark applications.

Currently, the following Apache Spark configurations are supported:
- `spark.driver.memory`
- `spark.driver.cores`
- `spark.executor.memory`
- `spark.executor.cores`
- `spark.driver.defaultJavaOptions`
- `spark.driver.extraLibaryPathe`
- `spark.driver.extraClassPath`

See [Spark Configuration](https://spark.apache.org/docs/latest/configuration.html){: external} to understand more about the Apache Spark properties.

Additionally, you can also specify default values for autoscaling Spark applications at the instance level. See [Autoscaling application configurations](/docs/AnalyticsEngine?topic=AnalyticsEngine-appl-auto-scaling) for details.

The following list shows the default values for Apache Spark configuration settings adapted for {{site.data.keyword.iae_full_notm}} instances:

- `"spark.driver.memory": "4G"`
- `"spark.driver.cores": "1"`
- `"spark.executor.memory": "4G"`
- `"spark.executor.cores": "1"`
- `"spark.eventLog.enabled": "true"`


For the default limits and quotas for {{site.data.keyword.iae_short}} instances and the supported Spark driver and executor vCPU and memory combinations, see [Limits and quotas for {{site.data.keyword.iae_short}} instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits).

When you create an instance you can override the open source default Apache Spark configuration settings. Note that you can specify or change configuration options after the instance was created. You can override or add settings by using the REST API. See [Spark application REST API](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api).

## Serverless instance features and execution methods

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
{: caption="Table 1. Supported serverless instance features by access role and execution methods" caption-side="top"}
{: #features-table-1}
{: row-headers}
