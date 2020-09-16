---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-15"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}


# FAQs about operations
{: #operations-faqs}

## How much time does it take for the cluster to get started?
{: #time-4-cluster-2-start}
{: faq}

When using the Spark software pack, a cluster takes about
7 to 9 minutes to be started and be ready to run applications. When using the Hadoop and Spark software pack, a cluster takes about 15 to 20 minutes to be started and be ready to run  applications.

## How can I access or interact with my cluster?
{: #how-access-cluster}
{: faq}

There are several interfaces which you can use to access the cluster.
- SSH
- Ambari console
- REST APIs
- Cloud Foundry CLI

## How do I get data into the cluster?
{: #get-data-on-cluster}
{: faq}

The recommended way to read data to a cluster for processing is from {{site.data.keyword.cos_full_notm}}. Upload your data to {{site.data.keyword.cos_full_notm}} (COS) and use COS, Hadoop or Spark APIs to read the data. If your use-case requires data to be processed directly on the cluster, you can use one of the following ways to ingest the data:
- SFTP
- WebHDFS
- Spark
- Spark-streaming
- Sqoop

For more information, see the [documentation](/docs/AnalyticsEngine?topic=AnalyticsEngine-upload-files-hdfs).

## How do I configure my cluster?
{: #how-2-configure-cluster}
{: faq}
{: support}

You can configure a cluster by using customization scripts or by directly modifying configuration parameters in the Ambari console. Customization scripts are a convenient way to define different
sets of configurations through a script, to spin up different types of clusters, or to use the same configuration repeatedly for repetitive jobs. You can find more information on cluster customization
[here](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-cluster).

## Can I stop or shutdown my {{site.data.keyword.iae_full_notm}} clusters to be charged on a per-use basis?
{: #charge-per-use}
{: faq}
{: support}

You are charged as long as the cluster is active and not on a per-use basis. For this reason, you should delete the instance after your job has completed and create a new instance before you start another job. To enable creating and deleting clusters as you need them however, means you must separate compute from storage. See [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#separate-compute-from-storage).

## Can I scale down nodes in existing {{site.data.keyword.iae_full_notm}} clusters?
{: #scale-down-nodes}
{: faq}
{: support}

No, you can't reduce the number of nodes in existing clusters; you can only add more nodes to those clusters. If you want to scale down, you must delete those clusters and create new ones with the correct number of nodes.

## Do I have root access in {{site.data.keyword.iae_full_notm}}?
{: #root-access}
{: faq}

No, users do not have sudo or root access to install privileges
because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS)  offering.

## Can I install my own Hadoop stack components?
{: #istall-hadoop-stack}
{: faq}

No, you cannot add components that are not supported by {{site.data.keyword.iae_full_notm}} because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS) offering. For example, you are not permitted to install a new Ambari Hadoop stack component through Ambari or otherwise. However, you can install non-server Hadoop ecosystem components, in other words, anything that can be installed and run in your user space is allowed.

## Which third party packages can I install?
{: #third-party-packages}
{: faq}

You can only install packages that are available in the CentOS repositories by using the `packageadmin` tool that comes with {{site.data.keyword.iae_full_notm}}. You do not require sudo or root privileges to install or run any packages from the CentOS repositories.

You should perform all cluster customization by using customization scripts at the time the cluster is started to ensure repeatability and consistency when creating further new clusters.

## Can I monitor the cluster?
{: #monitor-cluster}
{: faq}

Can I configure alerts? Ambari components can be monitored by using the built-in Ambari metrics alerts.

## How do I scale my cluster?
{: #scale-cluster}
{: faq}

You can scale a cluster by adding nodes to it. Nodes can be added through the {{site.data.keyword.iae_full_notm}} UI or by using the CLI tool.

## Can I scale my cluster while jobs are running on it?
{: #scale-while-jobs-run}
{: faq}

Yes, you can add new nodes to your cluster while jobs are still running. As soon as the new nodes are ready, they will be used to execute further steps of the running job.

## Can I adjust resource allocation in a Spark interactive application?
{: #adjust-resource-allocation-interactive-app}
{: faq}

If you need to run large Spark interactive jobs, you can adjust the kernel settings to tune resource allocation, for example, if your Spark container is too small for your input work load. To get the maximum performance from your cluster for a Spark job, see [Kernel settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings).

## Does the {{site.data.keyword.iae_full_notm}} operations team monitor and manage all service instances?
{: #dev-ops}
{: faq}
{: support}

Yes, the {{site.data.keyword.Bluemix_notm}} operations team ensures that all services are  running so that you can spin up clusters, submit jobs and manage  cluster lifecycles through the interfaces provided. You can monitor and manage your clusters by using the tools available in Ambari or additional services provided by {{site.data.keyword.iae_full_notm}}.

## Where are my job log files?
{: #job-log-files}
{: faq}
{: support}

For most components, the log files can be retrieved by using the Ambari GUI. Navigate to the respective component, click **Quick Links** and select the respective component GUI.  An alternative method is to SSH to the node where the component is running and access the `/var/log/<component>` directory.

## How can I debug a Hive query on {{site.data.keyword.iae_full_notm}}?
{: #debug-hive-query}
{: faq}

To debug a Hive query on {{site.data.keyword.iae_full_notm}}:

1. Open the Ambari console, and then on the dashboard, click **Hive > Configs > Advanced**.
2. Select **Advanced > hive-log4j** and change `hive.root.logger=INFO,RFA` to `hive.root.logger=DEBUG,RFA`.
3. Run the Hive query.
4. SSH to the {{site.data.keyword.iae_full_notm}} cluster. The Hive logs are located in `/tmp/clsadmin/hive.log`.

## More FAQs
{: #more-faqs-operations}

- [General FAQs](/docs/AnalyticsEngine?topic=AnalyticsEngine-general-faqs)
- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/AnalyticsEngine?topic=AnalyticsEngine-faqs-architecture)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-integration-faqs)
- [FAQs about {{site.data.keyword.iae_full_notm}} security](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-faqs)
