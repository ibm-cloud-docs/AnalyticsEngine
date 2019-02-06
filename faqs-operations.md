---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-06"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}


# FAQs about operations

## How much time does it take for the cluster to get started?
{: faq}

When using the Spark software pack, a cluster takes about
7 to 9 minutes to be started and be ready to run applications. When using the Hadoop and Spark software pack, a cluster takes about 15 to 20 minutes to be started and be ready to run  applications.

## How can I access or interact with my cluster?
{: faq}

There are several interfaces which you can use to access the cluster.
- SSH
- Ambari console
- REST APIs
- Cloud Foundry CLI

## How do I get data into the cluster?
{: faq}

The recommended way to read data to a cluster for processing is from {{site.data.keyword.Bluemix_notm}} Object Storage. Upload your data to {{site.data.keyword.Bluemix_notm}} Object Storage (COS) and use COS, Hadoop or Spark APIs to read the data. If your use-case requires data to be processed directly on the cluster, you can use one of the following ways to ingest the data:
- SFTP
- WebHDFS
- Spark
- Spark-streaming
- Sqoop

For more information, see the [documentation](https://{DomainName}/docs/services/AnalyticsEngine/Upload-files-to-HDFS.html#uploading-files-to-hdfs).

## How do I configure my cluster?
{: faq}

You can configure a cluster by using customization scripts or by directly modifying configuration parameters in the Ambari console. Customization scripts are a convenient way to define different
sets of configurations through a script, to spin up different types of clusters, or to use the same configuration repeatedly for repetitive jobs. You can find more information on cluster customization
[here](https://{DomainName}/docs/services/AnalyticsEngine/customizing-cluster.html#customizing-a-cluster).

## Do I have root access in {{site.data.keyword.iae_full_notm}}?
{: faq}

No, users do not have sudo or root access to install privileges
because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS)  offering.

## Can I install my own Hadoop stack components?
{: faq}

No, you cannot add components that are not supported by {{site.data.keyword.iae_full_notm}} because {{site.data.keyword.iae_full_notm}} is a Platform as a Service (PaaS) offering. For example, you are not permitted to install a new Ambari Hadoop stack component through Ambari or otherwise. However, you can install non-server Hadoop ecosystem components, in other words, anything that can be installed and run in your user space is allowed.

## Which third party packages can I install?
{: faq}

You can install packages which are available in the CentOS repo by using the `packageadmin` tool that comes with {{site.data.keyword.iae_full_notm}}. Libraries or packages (for example, for Python or R) that can be installed and run in your user space are allowed. You do not require sudo or root privileges to install or run any packages from non-CentOS repositories or RPM package management systems.
You should perform all cluster customization by using customization
scripts at the time the cluster is started to ensure repeatability and consistency when creating further new clusters.

## Can I monitor the cluster?
{: faq}

Can I configure alerts? Ambari components can be monitored by  using the built-in Ambari metrics alerts.

## How do I scale my cluster?
{: faq}

You can scale a cluster by adding nodes to it. Nodes can be added through the {{site.data.keyword.iae_full_notm}} UI or by using the CLI tool.

## Can I scale my cluster while jobs are running on it?
{: faq}

Yes, you can add new nodes to your cluster while jobs are still running. As soon as the new nodes are ready, they will be used to execute further steps of the running job.

## Can I adjust resource allocation in a Spark interactive application?
{: faq}

If you need to run large Spark interactive jobs, you can adjust the kernel settings to tune resource allocation, for example, if your Spark container is too small for your input work load. To get the maximum performance from your cluster for a Spark job, see [Kernel settings](Kernel-Settings.html).

## Does the {{site.data.keyword.iae_full_notm}} operations team monitor and manage all service instances?
{: faq}

Yes, the {{site.data.keyword.Bluemix_notm}} operations team ensures that all services are  running so that you can spin up clusters, submit jobs and manage  cluster lifecycles through the interfaces provided. You can monitor and manage your clusters by using the tools available in Ambari or additional services provided by {{site.data.keyword.iae_full_notm}}.

## Where are my job log files?
{: faq}

For most components, the log files can be retrieved by using the Ambari GUI. Navigate to the respective component, click **Quick Links** and select the respective component GUI.  An alternative method is to ssh to the node where the component is running and access the `/var/log/<component>` directory.

## How can I debug a Hive query on {{site.data.keyword.iae_full_notm}}?
{: faq}

To debug a Hive query on {{site.data.keyword.iae_full_notm}}:

1. Open the Ambari console, and then on the dashboard, click **Hive > Configs > Advanced**.
2. Select **Advanced > hive-log4j** and change `hive.root.logger=INFO,RFA` to `hive.root.logger=DEBUG,RFA`.
3. Run the Hive query.
4. SSH to the {{site.data.keyword.iae_full_notm}} cluster. The Hive logs are located in `/tmp/clsadmin/hive.log`.

## More FAQs

- [General FAQs](/docs/services/AnalyticsEngine/faqs-general.html)
- [FAQs about the {{site.data.keyword.iae_full_notm}} architecture](/docs/services/AnalyticsEngine/faqs-architecture.html)
- [FAQs about {{site.data.keyword.iae_full_notm}} integration](/docs/services/AnalyticsEngine/faqs-integration.html)
- [FAQs about {{site.data.keyword.iae_full_notm}} security](/docs/services/AnalyticsEngine/faqs-security.html)
