---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-29"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Differences between classic and serverless instances
{: #differences-between-instances}

The following table lists the key differences between classic {{site.data.keyword.iae_short}} and {{site.data.keyword.iae_short}} Serverless Spark instances by feature.

| Feature |	Classic {{site.data.keyword.iae_short}} |	{{site.data.keyword.iae_short}} Serverless Spark |
|---------|--------------------------|------------------------------------|
| Product Stack |	Hortonworks Data Platform (HDP), with Hadoop, Yarn, Spark, Hive, HBase, Oozie | Apache Spark (open source). See [Key concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#key-concepts). |
| Spark Resource Manager | Yarn based | Standalone |
| Spark version | 2.3 | 3.1 (as of today). See [Key concepts](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#key-concepts). |
| Pre-installed packages | Comes with a set of pre-installed Python, R and scala packages | Comes with a set of pre-installed Python, R and scala packages |
| Lifecycle of a Spark cluster | The {{site.data.keyword.iae_short}} HDP cluster exists as long as service instance is alive. | Created on demand. When a Spark application is submitted, a Spark cluster is created on the fly. The cluster is not directly accessible to user. |
| Service instance | One HDP based cluster is associated with one instance | The 	service instance houses the details of the `instance home`. Multiple Spark workloads can be executed against this one instance. See [Instance architecture](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#serverless-architecture). |
| SSH access to cluster |	Can SSH/SCP to the cluster | No SSH access to the cluster |
| User management | Single user system through `clsadmin` | {{site.data.keyword.cloud_notm}} IAM based access. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless). |
| UI access | Ambari UI, Spark UI, and Spark History. You can navigate to the UI from Yarn/Spark | Ambari is no longer available. In the upcoming release, the Spark UI & Spark History will be accessible. You can then navigate to the UI from the {{site.data.keyword.cloud_notm}} console against the application. See [Managing instances through the {{site.data.keyword.cloud_notm}} console](/docs/AnalyticsEngine?topic=AnalyticsEngine-manage-serverless-console). |
| Cluster management | Ambari and {{site.data.keyword.iae_short}} REST APIs |- {{site.data.keyword.cloud_notm}} console: see [Managing using the {{site.data.keyword.cloud_notm}} console](/docs/AnalyticsEngine?topic=AnalyticsEngine-manage-serverless-console)  \n- REST APIs and CLI: see [Retrieving details of a serverless instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-instance-details) |
| Customization | At cluster level | At instance level. After customization is done at instance level, it can take effect across workloads. See [Customization overview](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-instance). |
| Billing frequency | Hourly or monthly, depending on service plan | Per second billing |
| Local distributed storage | HDFS | No HDFS |
| Source of data for analytics | {{site.data.keyword.cos_full_notm}} | {{site.data.keyword.cos_full_notm}}. See [Using {{site.data.keyword.cos_short}} as the instance home](/docs/AnalyticsEngine?topic=AnalyticsEngine-cos-serverless). |
| Application details for {{site.data.keyword.cos_short}} authentication |	Supports both IAM API Key and HMAC | Supports both IAM API Key and HMAC. See [What are the {{site.data.keyword.cos_short}} credentials](/docs/AnalyticsEngine?topic=AnalyticsEngine-cos-serverless#what-are-cos-creds). |
| Home instance {{site.data.keyword.cos_short}} authentication | N/A | Supports only HMAC. Note that using the API Key in the instance home details specification will work, however currently no logs will be forwarded to the configured {{site.data.keyword.la_full_notm}} instance. So using the API key might not work for most users. |
| {{site.data.keyword.cos_short}} endpoint  \n This applies to both the instance home and datasource for analytics endpoint. | Supports only private and public endpoints. It is more efficient to use private endpoints. | Supports public and direct endpoints.|
| Livy API | Livy API | Livy-like API. No sessions API, only batch API. No Livy API to fetch logs. See [Livy batch APIs](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api-serverless). |
| Included libraries | Parquet modular encryption, geo-spatio, time series, data skipping | Parquet modular encryption, geo-spatio, time series, data skipping |
| {{site.data.keyword.la_short}} integration | Supported with explicit specification of a {{site.data.keyword.la_short}} ingestion key | Supports forwarding logs from the {{site.data.keyword.iae_full_notm}} service to an {{site.data.keyword.la_full_notm}} instance that was enabled to receive platform logs. See [Configuring and viewing logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-viewing-logs). |
| CLI | `ibmcloud ae` or `ibmcloud analytics-engine`| `ibmcloud ae-v3` or `ibmcloud analytics-engine-v3`. See [{{site.data.keyword.iae_short}} CLI](/docs/analytics-engine-cli-plugin?topic=analytics-engine-cli-plugin-CLI_analytics_engine). |
| Driver/executor sizes | Can be customized | T-shirt sizes: see [Supported Spark driver and executor vCPU and memory combinations](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits#cpu-mem-combination) |
| Spark streaming | Checkpointing can be done for {{site.data.keyword.cos_short}}/HDFS | Checkpointing can be done only for {{site.data.keyword.cos_short}}. Make sure to run streaming applications only for 3 days to accommodate for AnalyticsEngine maintenance. |
| Limits and Quotas | Limit of 20 nodes per cluster | 5 instances per account with limits on cores and cpu per instance. See [Application limits](/docs/AnalyticsEngine?topic=AnalyticsEngine-limits#limits_application). |

## Next steps

- [Migrating from classic instances: instance creation](/docs/AnalyticsEngine?topic=AnalyticsEngine-instance-creation)
- [Migrating from classic instances: get service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-get-service-endpoints)
- [Migrating from classic instances: Spark submit](/docs/AnalyticsEngine?topic=AnalyticsEngine-migrate-spark-submit)
- [Migrating from classic instances: livy API](/docs/AnalyticsEngine?topic=AnalyticsEngine-migrate-livy)
- [Migrating from classic instances: customization](/docs/AnalyticsEngine?topic=AnalyticsEngine-migrate-customization)


<!-- Supports only direct and public endpoints. It is more efficient to use direct endpoints.  See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless). -->
