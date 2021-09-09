---

copyright:
  years: 2017, 2021
lastupdated: "2021-07-20"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Accessing the Spark history server

The Spark history server is a web UI where you can view the status of running and completed Spark applications on a provisioned serverless instance. If you want to analyze how different stages of your Spark application performed, you can view the details in the Spark history server UI.

The history server shows only the running and the completed or completed but failed Spark applications. It doesn't show the Spark applications that couldn't be started because, for example, the wrong arguments were passed or the number of passed arguments is less than the expected number.

To access the link to the Spark history server from an application for your provisioned serverless instance:

1. Open your resource list on [IBM Cloud](https://cloud.ibm.com/resources).
1. Click **Services and software** and select your instance.
1. Select your instance, click the **Applications** tab.
1. Select your submitted application, click the Actions menu at the far right and select **Launch UI**.
1. Stop the history server when you no longer need it to release unnecessary resources.
