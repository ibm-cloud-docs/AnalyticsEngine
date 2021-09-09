---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-02"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}
{:support: data-reuse='support'}


# FAQs
{: #faqs-serverless}

## What is {{site.data.keyword.iae_full_notm}} serverless?
{: #what-is-iae-serverless}
{: faq}
{: support}

The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark offers a new consumption model using Apache Spark. An {{site.data.keyword.iae_short}} serverless instance does not consume any resources when no workloads are running. When you submit Spark applications, Spark clusters are created in seconds and are spun down as soon as the applications finish running. You can develop and deploy Spark SQL, data transformation, data science, or machine learning jobs using the Spark  application API.

## What are the advantages of {{site.data.keyword.iae_full_notm}} serverless instances?
{: #advantages-serverless}
{: faq}

With {{site.data.keyword.iae_full_notm}} serverless, compute and memory resources are allocated on demand when Spark workloads are deployed. When an application is not in running state, no computing resources are allocated to the {{site.data.keyword.iae_full_notm}} serverless instance. Pricing is based on the actual amount of resources consumed by the instance, billed on a per second basis.

## Does the {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark support Hadoop?
{: #hadoop-serverless}
{: faq}
{: support}

No, currently, the {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark only supports Apache Spark.

## Can I change the instance home storage of a serverless instance?
{: #change-instance-home}
{: faq}
{: support}

No, you can't. After an instance home storage is associated with an {{site.data.keyword.iae_full_notm}} serverless instance, it cannot be changed because instance home contains all instance relevant data, such as the Spark events and custom libraries. Changing instance home would result in the loss of the Spark history data and custom libraries.

## How is user management and access control managed in a serverless instance?
{: #user-management}
{: faq}
{: support}

User management and access control of an {{site.data.keyword.iae_full_notm}} serverless instance and its APIs is done through {{site.data.keyword.Bluemix_short}} Identity and Access Management (IAM). You  use IAM access policies to invite users to collaborate on your instance and grant them the necessary privileges. See [Granting permissions to users](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions-serverless).

## How do I define the size of the cluster to run my Spark application?
{: #cluster-size-serverless}
{: faq}
{: support}

You can specify the size of the cluster either at the time the instance is created or when submitting Spark applications. You can choose the CPU and memory requirements of your Spark driver and executor, as well the number of executors if you know those requirements up-front. Alternatively, you can choose to let the {{site.data.keyword.iae_full_notm}} service autoscale the Spark cluster based on the application's demand. To override default Spark configuration settings at instance creation or when submitting an application, see [Default Spark configuration](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config). For details on autoscaling, [Enabling application autoscaling](/docs/AnalyticsEngine?topic=AnalyticsEngine-appl-auto-scaling).

## How do I install custom libraries to my serverless instance?
{: #install-cust-libs}
{: faq}
{: support}

You can use custom libraries in Python, R, Scala or Java and make them available to your Spark application by creating a library set and referencing it in your application at the time you submit the Spark application. See [Creating a library set](/docs/AnalyticsEngine?topic=AnalyticsEngine-create-lib-set).

## How can the serverless instance be monitored?
{: #monitor-serverless-instances}
{: faq}
{: support}

Currently, you can monitor Spark applications in the following ways:

- By tracking the state of the Spark application. For details, see [Getting the state of a submitted application](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-app-rest-api#spark-app-status).
- By viewing the Spark history events that are forwarded to the {{site.data.keyword.cos_full_notm}} instance that you specified as instance home. You can download these events and view them in the Spark history server UI installed on your desktop. At a later stage, you will be able to launch Spark history server UI from within the {{site.data.keyword.iae_full_notm}} service instance details UI page.

## How do I set up autoscaling policies for my serverless instance?
{: #autoscaling-serverless}
{: faq}
{: support}

You can enable autoscaling for all applications at instance level at the time  you create an instance of the {{site.data.keyword.iae_short}} Standard serverless plan for Apache Spark or per application at the time you submit the application. For details, see [Enabling application autoscaling](/docs/AnalyticsEngine?topic=AnalyticsEngine-appl-auto-scaling).   

## Can I connect to a serverless instance with the Apache Livy API?
{: #use-livy-api}
{: faq}

Yes, the {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark provides an API interface similar to Livy batch API. For details, see [Livy API](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api).   

## Where can I find the logs for my Spark applications?
{: #logs-serverless}
{: faq}
{: troubleshoot}

You can aggregate the logs from your Spark applications to {{site.data.keyword.la_short}}. For details, see [Configuring and viewing logs](/docs/AnalyticsEngine?topic=AnalyticsEngine-viewing-logs).

## How can I track actions performed by users on a serverless Spark instance?
{: #track-actions}
{: faq}
{: troubleshoot}

You can use the {{site.data.keyword.at_short}} service to track how users and applications interact with {{site.data.keyword.iae_full_notm}} in {{site.data.keyword.cloud}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. See [Auditing events for {{site.data.keyword.iae_full_notm}} serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-at_events-serverless).
