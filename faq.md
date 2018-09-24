---

copyright:
  years: 2017,2018
lastupdated: "2018-09-24"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Frequently asked questions

<ul>
<li><b>General</b>
<ul>
<li>[What is {{site.data.keyword.iae_full_notm}}?](#what-is-ibm-analytics-engine-)</li>
<li>[How is an {{site.data.keyword.iae_full_notm}} cluster different from a regular Hadoop cluster?](#how-is-an-ibm-analytics-engine-cluster-different-from-a-regular-hadoop-cluster-)</li>
<li>[How do I get started with {{site.data.keyword.iae_full_notm}}?](#how-do-i-get-started-with-ibm-analytics-engine-)</li>
<li>[Which distribution is used in {{site.data.keyword.iae_full_notm}}?](#which-distribution-is-used-in-ibm-analytics-engine-)</li>
<li>[Which HDP components are supported in {{site.data.keyword.iae_full_notm}}?](#which-hdp-components-are-supported-in-ibm-analytics-engine-)</li>
<li>[What node sizes are available in IBM Analytics  Engine?](#what-node-sizes-are-available-in-ibm-analytics-engine-)</li>
<li>[Why is there so little HDFS space on the clusters?](#why-is-there-so-little-hdfs-space-on-the-clusters-)</li>
<li>[How many {{site.data.keyword.iae_full_notm}} clusters can I spin up?](#how-many-ibm-analytics-engine-clusters-can-i-spin-up-)</li>
<li>[Is there a free usage tier to try {{site.data.keyword.iae_full_notm}}?](#is-there-a-free-usage-tier-to-try-ibm-analytics-engine-)</li>
<li>[How does the Lite plan  work?](#how-does-the-lite-plan-work-)</li>
</ul>
</li>
<li><b>Architecture</b>
<ul>
<li>[Is {{site.data.keyword.Bluemix_notm}} Object Storage included in {{site.data.keyword.iae_full_notm}}?](#is-ibm-cloud-object-storage-included-in-ibm-analytics-engine-)</li>
<li>[How does {{site.data.keyword.Bluemix_notm}} Object Storage work in the {{site.data.keyword.iae_full_notm}} Hadoop  environment?](#how-does-ibm-cloud-object-storage-work-in-the-ibm-analytics-engine-hadoop-environment-)
<li>[What other components like  Object Storage should I consider while designing a solution using {{site.data.keyword.iae_full_notm}}?](#what-other-components-like-ibm-cloud-object-storage-should-i-consider-while-designing-a-solution-using-ibm-analytics-engine-)</li>
<li>[How should I size my  cluster?](#how-should-i-size-my-cluster-)</li>
<li>[How do I design and size multiple environments for different purposes?](#how-do-i-design-and-size-multiple-environments-for-different-purposes-)</li>
<li>[How is user management done in {{site.data.keyword.iae_full_notm}}?](#how-is-user-management-done-in-ibm-analytics-engine-)</li>
<li>[How is data access control enforced in {{site.data.keyword.iae_full_notm}}?](#how-is-data-access-control-enforced-in-ibm-analytics-engine-)</li>
<li>[Can I run a cluster or job for a long time?](#can-i-run-a-cluster-or-job-for-a-long-time-)</li>
</ul>
<li><b>Operations</b>
<ul>
<li>[How much time does it take for the cluster to get started?](#how-much-time-does-it-take-for-the-cluster-to-get-started-)</li>
<li>[How can I access or interact with my cluster?](#how-can-i-access-or-interact-with-my-cluster-)</li>
<li>[How do I get data into the  cluster?](#how-do-i-get-data-into-the-cluster-)</li>
<li>[How do I configure my cluster?](#how-do-i-configure-my-cluster-)</li>
<li>[Do I have root access in IBM Analytics  Engine?](#do-i-have-root-access-in-ibm-analytics-engine-)</li>
<li>[Can I install my own Hadoop stack  components?](#can-i-install-my-own-hadoop-stack-components-)</li>
<li>[Which third party packages can I  install?](#which-third-party-packages-can-i-install-)</li>
<li>[Can I monitor the cluster?](#can-i-monitor-the-cluster-)</li>
<li>[How do I scale my cluster?](#how-do-i-scale-my-cluster-)</li>
<li>[Can I scale my cluster while jobs are running on  it?](#can-i-scale-my-cluster-while-jobs-are-running-on-it-)</li>
<li>[Can I adjust resource allocation in a Spark interactive application?](#can-i-adjust-resource-allocation-in-a-spark-interactive-application-)
<li>[Does the {{site.data.keyword.iae_full_notm}} operations team monitor and manage all service  instances?](#does-the-ibm-analytics-engine-operations-team-monitor-and-manage-all-service-instances-)</li>
<li>[Where are my job log files?](#where-are-my-job-log-files-)</li>
</ul>
</li>
<li><b>Security</b>
<ul>
<li>[What type of encryption is supported?](#what-type-of-encryption-is-supported-)</li>
<li>[Which ports are open on the public interface on the  cluster?](#which-ports-are-open-on-the-public-interface-on-the-cluster-)</li>
</ul>
</li>
<li><b>Integration</b>
<ul>
<li>[Which other {{site.data.keyword.Bluemix_notm}} services can I use with {{site.data.keyword.iae_full_notm}}?](#which-other-ibm-cloud-services-can-i-use-with-ibm-analytics-engine-)</li>
<li>[How is {{site.data.keyword.iae_full_notm}} integrated with {{site.data.keyword.DSX_short}}?](#how-is-ibm-analytics-engine-integrated-with-ibm-watson-studio-)</li>
<li>[Can I use Kafka for data  ingestion?](#can-i-use-kafka-for-data-ingestion-)</li>
<li>[Can I set ACID properties for Hive in {{site.data.keyword.iae_full_notm}}?](#can-i-set-acid-properties-for-hive-in-ibm-analytics-engine-)</li>
</ul></li>
</ul>

## General

### What is {{site.data.keyword.iae_full_notm}}?

{{site.data.keyword.iae_full_notm}} provides a flexible framework to develop and deploy analytics applications on Hadoop and Spark. It allows you to spin up Hadoop and Spark clusters and manage them through their lifecycle.

### How is an {{site.data.keyword.iae_full_notm}} cluster different from a regular Hadoop cluster?

{{site.data.keyword.iae_full_notm}} is based on an architecture which separates compute and storage. In a traditional Hadoop architecture, the cluster is used to both store data and perform application processing. In {{site.data.keyword.iae_full_notm}}, storage and compute are separated. The cluster is used for running applications and {{site.data.keyword.Bluemix_notm}} Object Storage for persisting the data. The benefits of such an architecture  include flexibility, simplified operations, better  reliability and cost effectiveness. Read this [whitepaper](https://www-01.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=ASW12451USEN&) to learn more.

### How do I get started with {{site.data.keyword.iae_full_notm}}?

{{site.data.keyword.iae_full_notm}} is available on {{site.data.keyword.Bluemix_notm}}. Follow this [link](https://console.bluemix.net/docs/services/AnalyticsEngine/getting-started.html#getting-started) to learn more about the service and to start using it. You will also find tutorials and code samples to get you off to a fast start.

### Which distribution is used in {{site.data.keyword.iae_full_notm}}?
{{site.data.keyword.iae_full_notm}} is based on open source Hortonworks Data Platform (HDP). To find the currently supported version see the  [documentation](https://console.bluemix.net/docs/services/AnalyticsEngine/index.html#introduction).

### Which HDP components are supported in {{site.data.keyword.iae_full_notm}}?
To see the full list of supported components and versions, see the [documentation](https://console.bluemix.net/docs/services/AnalyticsEngine/index.html#introduction).

### What node sizes are available in {{site.data.keyword.iae_full_notm}}?
To see the currently supported node sizes, see the [documentation](https://console.bluemix.net/docs/services/AnalyticsEngine/index.html#introduction).

### Why is there so little HDFS space on the clusters?

What if I want to run a cluster that has a lot of data to be processed at one time?

The clusters in {{site.data.keyword.iae_full_notm}} are intended to be used as a compute clusters and not as persistent storage for data. Data should be persisted in [{{site.data.keyword.Bluemix_notm}} Object Storage](https://www.ibm.com/cloud/object-storage). This provides a more flexible, reliable, and cost effective way to build analytics applications. See this [whitepaper](https://www-01.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=ASW12451USEN&) to learn more about this topic. The Hadoop Distributed File System (HDFS) should be used at most only for intermediate storage during
processing. All final data (or even intermediate data) should be written to Cloud Object Storage before the cluster is deleted. If your intermediate storage requirements exceed the HDFS space  available on a node, you can add more nodes to the cluster.

### How many {{site.data.keyword.iae_full_notm}} clusters can I spin up?

There is no limit to the number of clusters you can spin up.

### Is there a free usage tier to try {{site.data.keyword.iae_full_notm}}?

Yes, we provide the Lite plan which can be used free of charge. Apart from this, as a new {{site.data.keyword.Bluemix_notm}} user, you are also entitled to $200 in credit that can be used against {{site.data.keyword.iae_full_notm}} or any service on {{site.data.keyword.Bluemix_notm}}.

### How does the Lite plan work?
The Lite plan provides 50 node-hours of free {{site.data.keyword.iae_full_notm}} usage. One cluster can be provisioned every 30  days. After the 50 node-hours are exhausted, you can  upgrade to a paid plan within 24 hours to continue using the same cluster. If you do not upgrade within 24 hours, the cluster will be deleted and you have to provision a new one after the 30 day limit has passed.

Depending on the size of your cluster, actual hours of use might vary. For instance, a cluster with 1 master and 3 data nodes (4  nodes in total) will run for 12.5 hours on the clock (50 hours/4 nodes). However, a cluster with 1 master and 1 data node (2 nodes in total) will run for 25 hours on the clock (50 hours/2 nodes). The node-hours cannot be paused, for example, you cannot use 10 node-hours, pause, and then come back and use the remaining 40 node-hours.

## Architecture

### Is {{site.data.keyword.Bluemix_notm}} Object Storage included in {{site.data.keyword.iae_full_notm}}?

No, {{site.data.keyword.Bluemix_notm}} Object Storage is not included. It is a separate offering. To learn more about {{site.data.keyword.Bluemix_notm}} Object Storage, see the [product documentation](https://console.bluemix.net/docs/services/cloud-object-storage/about-cos.html#about-ibm-cloud-object-storage) or the  [documentation about its  functionality](https://www.ibm.com/cloud/object-storage).

### How does {{site.data.keyword.Bluemix_notm}} Object Storage work in the {{site.data.keyword.iae_full_notm}} Hadoop environment?
Is it exactly equivalent to HDFS, only that it uses a different URL?

{{site.data.keyword.Bluemix_notm}} Object Storage implements most of the Hadoop File System interface. For simple read and write operations, applications that use the Hadoop File System API will continue to work when HDFS is substituted by {{site.data.keyword.Bluemix_notm}} Object Storage. Both are high performance storage options that are fully supported by Hadoop.

### What other components like {{site.data.keyword.Bluemix_notm}} Object Storage should I  consider while designing a solution using {{site.data.keyword.iae_full_notm}}?

In addition to {{site.data.keyword.Bluemix_notm}} Object Storage, consider using Compose  MySQL, available on {{site.data.keyword.Bluemix_notm}}, for persisting Hive metadata. When you delete a cluster, all data and metadata is lost. Persisting
Hive metadata in an external relational store like Compose allows  you to reuse this data again after clusters were deleted or access to clusters was denied.

{{site.data.keyword.iae_full_notm}} supports passing the location of metadata through customization scripts which you can use when starting a cluster. Hence, you can have the cluster pointing to the right metadata location as soon as it is spun up.

### How should I size my cluster?
Sizing a cluster is highly dependent on workloads. Here are some general guidelines:

For Spark workloads reading data from the object store, the minimum RAM in a cluster should be at least half the size of the data you want to analyze in any given job. For the best results, the recommended sizing for Spark workloads reading data from the object store is to have the RAM twice the size of the data you want to analyze. If you expect to have a lot of intermediate data, you should size the number of nodes to provide the right amount of HDFS space in the cluster.

### How do I design and size multiple environments for different purposes?

If you want to size multiple environments, for example a production environment with HA, a disaster recovery environment, a staging environment with HA, and a development environment, you need to consider the following aspects.

Each of these environments should use a separate cluster. If
you have multiple developers on your team, consider a separate
cluster for each developer unless they can share the same cluster credentials. For a development environment, generally, a cluster with  1 master and 2 compute nodes should suffice. For a staging environment where functionality is tested, a cluster with 1 master and 3 compute nodes is recommended. This gives you additional resources to test on a slightly bigger scale before deploying to production. For a disaster recovery environment with more than one cluster, you will need third party remote data replication capabilities.

Because data is persisted in Cloud Object Storage in {{site.data.keyword.iae_full_notm}}, you do need to have more than one cluster  running all the time. If the production cluster goes down, then a new cluster can be spun up using the DevOps tool chain and can be designated as the production cluster. You should use the customization scripts to configure the new cluster exactly like the previous production cluster.

### How is user management done in {{site.data.keyword.iae_full_notm}}?
How do I add more users to my cluster?

All clusters in {{site.data.keyword.iae_full_notm}} are single user, in other words, each cluster has only one Hadoop user ID with which all jobs
are executed. User authentication and access control is managed by the {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) service. After a user has logged on to {{site.data.keyword.Bluemix_notm}}, access to {{site.data.keyword.iae_full_notm}} is given or denied based on the IAM permissions set by the administrator.

A user can share his or her cluster’s user ID and password with other users; note however that in this case the other users have full access to the cluster. Sharing a cluster through a project in {{site.data.keyword.DSX_short}} is the recommended approach. In this scenario, an administrator sets up the cluster through the {{site.data.keyword.Bluemix_notm}} portal and *associates* it with a project in {{site.data.keyword.DSX_short}}. After this is done, users who have been granted access to that project can submit jobs through notebooks or other tools that requires a Spark or Hadoop runtime. An advantage of this approach is that user access to the {{site.data.keyword.iae_full_notm}} cluster or to any data to be analyzed can be controlled within {{site.data.keyword.DSX_short}}.

### How is data access control enforced in {{site.data.keyword.iae_full_notm}}?

Data access control can be enforced by using {{site.data.keyword.Bluemix_notm}} Object Storage ACLs (access control lists). ACLs in {{site.data.keyword.Bluemix_notm}} Object Storage are tied to the {{site.data.keyword.Bluemix_notm}} Identity and Access Management service.

An administrator can set permissions on a Cloud Object Storage bucket or on stored files. Once these permissions are set, the credentials of a user determine whether access to a data object through {{site.data.keyword.iae_full_notm}} can be granted or not.

In addition, all data in Cloud Object Storage can be cataloged using the IBM Watson Knowledge Catalog functionality. Governance policies can be defined and enforced using Data Catalog after the data was cataloged. Projects created in WDP can be used for a better management of user access control.

### Can I run a cluster or job for a long time?

Yes, you can run a cluster for as long as is required. However, to prevent data loss in case of an accidental cluster failure, you  should ensure that data is periodically written to {{site.data.keyword.Bluemix_notm}} Object Storage and that you don't use HDFS as a persistent store.

## Operations

### How much time does it take for the cluster to get started?

When using the Spark software pack, a cluster takes about
7 to 9 minutes to be started and be ready to run applications. When using the Hadoop and Spark software pack, a cluster takes about 15 to 20 minutes to be started and be ready to run  applications.

### How can I access or interact with my cluster?

There are several interfaces which you can use to access the cluster.
- SSH
- Ambari Console
- REST APIs
- Cloud Foundry CLI

### How do I get data into the cluster?

The recommended way to read data to a cluster for processing is from {{site.data.keyword.Bluemix_notm}} Object Storage. Upload your data to {{site.data.keyword.Bluemix_notm}} Object Storage (COS) and use COS, Hadoop or Spark APIs to read the data. If your use-case requires data to be processed directly on the cluster, you can use one of the following ways to ingest the data:
- SFTP
- WebHDFS
- Spark
- Spark-streaming
- Sqoop

For more information, see the [documentation](https://console.bluemix.net/docs/services/AnalyticsEngine/Upload-files-to-HDFS.html#uploading-files-to-hdfs).

### How do I configure my cluster?

You can configure a cluster by using customization scripts or by directly modifying configuration parameters in the Ambari console. Customization scripts are a convenient way to define different
sets of configurations through a script, to spin up different types of clusters, or to use the same configuration repeatedly for repetitive jobs. You can find more information on cluster customization
[here](https://console.bluemix.net/docs/services/AnalyticsEngine/customizing-cluster.html#customizing-a-cluster).

### Do I have root access in {{site.data.keyword.iae_full_notm}}?
No, users do not have sudo or root access to install privileges
because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS)  offering.

### Can I install my own Hadoop stack components?
No, you cannot add components that are not supported by {{site.data.keyword.iae_full_notm}} because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS) offering. For example, you are not permitted to install a new Ambari Hadoop stack component through Ambari or otherwise. However, you can install non-server Hadoop ecosystem components, in other words, anything that can be installed and run in your user space is allowed.

### Which third party packages can I install?

You can install packages which are available in the CentOS repo by using the `packageadmin` tool that comes with {{site.data.keyword.iae_full_notm}}. Libraries or packages (for example, for Python or R) that can be installed and run in your user space are allowed. You do not require sudo or root privileges to install or run any packages from non-CentOS repositories or RPM package management systems.
You should perform all cluster customization by using customization
scripts at the time the cluster is started to ensure repeatability and consistency when creating further new clusters.

### Can I monitor the cluster?

Can I configure alerts? Ambari components can be monitored by  using the built-in Ambari metrics alerts (in the `Hadoop and Spark` pack).

### How do I scale my cluster?

You can scale a cluster by adding nodes to it. Nodes can be added through the {{site.data.keyword.iae_full_notm}} UI or by using the CLI tool.

### Can I scale my cluster while jobs are running on it?

Yes, you can add new nodes to your cluster while jobs are still running. As soon as the new nodes are ready, they will be used to execute further steps of the running job.

### Can I adjust resource allocation in a Spark interactive application?

If you need to run large Spark interactive jobs, you can adjust the kernel settings to tune resource allocation, for example, if your Spark container is too small for your input work load. To get the maximum performance from your cluster for a Spark job, see [Kernel settings](Kernel-Settings.html).

### Does the {{site.data.keyword.iae_full_notm}} operations team monitor and manage all service instances?

Yes, the {{site.data.keyword.Bluemix_notm}} operations team ensures that all services are  running so that you can spin up clusters, submit jobs and manage  cluster lifecycles through the interfaces provided. You can monitor and manage your clusters by using the tools available in Ambari or additional services provided by {{site.data.keyword.iae_full_notm}}.

### Where are my job log files?

For most components, the log files can be retrieved by using the Ambari GUI. Navigate to the respective component, click **Quick Links** and select the respective component GUI.  An alternative method is to ssh to the node where the component is running and access the `/var/log/<component>` directory.

## Security

### What type of encryption is supported?
Hadoop transparent data encryption is automatically enabled for each cluster. The cluster comes with a predefined HDFS encryption zone, which is identified by the HDFS path `/securedir`. Files
that are placed in the encryption zone are automatically  encrypted. The files are automatically decrypted when they are accessed through various Hadoop client applications, such as HDFS
shell commands, WebHDFS APIs, and the Ambari file browser. More information is available in the [documentation](https://console.bluemix.net/docs/services/AnalyticsEngine/Upload-files-to-HDFS.html).

All data on Cloud Object Storage is encrypted at-rest. You can use a private, encrypted endpoint available from Cloud Object Storage to  transfer data between Cloud Object Storage and {{site.data.keyword.iae_full_notm}} clusters. Any data that passes over the public facing ports (8443,22 and 9443) is encrypted.

### Which ports are open on the public interface on the cluster?

The following ports are open on the public interface on the
cluster:

- Port 8443 Knox
- Port 22 SSH
- Port 9443 Ambari

## Integration

### Which other {{site.data.keyword.Bluemix_notm}} services can I use with {{site.data.keyword.iae_full_notm}}?
{{site.data.keyword.iae_full_notm}} is a compute engine offered in {{site.data.keyword.DSX_full}} and can be used to push {{site.data.keyword.DSX_short}} jobs to {{site.data.keyword.iae_full_notm}}. Data can be written to Cloudant or Db2 Warehouse on Cloud after being processed by using Spark.

### How is {{site.data.keyword.iae_full_notm}} integrated with IBM Watson Studio?

{{site.data.keyword.iae_full_notm}} is a first class citizen in {{site.data.keyword.DSX_full}}. Projects (or individual notebooks) in
{{site.data.keyword.DSX_short}} can be associated with {{site.data.keyword.iae_full_notm}}. Once you have an
IBM Analytics cluster running in {{site.data.keyword.Bluemix_notm}}, log in to {{site.data.keyword.DSX_short}} using the same {{site.data.keyword.Bluemix_notm}} credentials you used for {{site.data.keyword.iae_full_notm}}, create a project, go to the project's Settings page, and then add  the {{site.data.keyword.iae_full_notm}} service instance you created to the  project. For details, including videos and tutorials, see [IBM Watson Learning ](https://developer.ibm.com/clouddataservices/docs/analytics-engine/get-started/).
After you have added the {{site.data.keyword.iae_full_notm}} service to the project, you can select to run a notebook on the service. For details on how to run code in a notebook, see [Code and run notebooks](https://dataplatform.ibm.com/docs/content/analyze-data/code-run-notebooks.html?audience=wdp&context=analytics).

### Can I use Kafka for data ingestion?

IBM Message Hub, an {{site.data.keyword.Bluemix_notm}} service is based on Apache Kafka. It can be used to ingest data to an object store. This data can then be analyzed on an {{site.data.keyword.iae_full_notm}} cluster. Message Hub can also integrate with Spark on the {{site.data.keyword.iae_full_notm}} cluster to bring data directly to the cluster.

### Can I set ACID properties for Hive in {{site.data.keyword.iae_full_notm}}?

Hive is not configured to support concurrency. Although you can  change the Hive configuration on {{site.data.keyword.iae_full_notm}} clusters, it is your responsibility that the cluster functions correctly after you have made any such changes.
