---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:support: data-reuse='support'}

# Troubleshooting serverless instances
{: #troubleshooting-serverless}

In this topic, you can find the answers to common questions about how to troubleshoot {{site.data.keyword.iae_full_notm}} serverless instances.

## Running ae-v3 CLI commands results in "missing-required-flags-error"
{: #missing-flags-error}
{: troubleshoot}
{: support}

When you run `ae-v3` CLI commands, you might see a "missing-required-flags-error".

For example:

```
ibmcloud ae-v3 spark-app status b5e12891-c160-4674-8b94-e2cc42722be3
FAILED
Error executing command::
missing-required-flags-error
```

The reason might be that you are using an older version of the `ae-v3` CLI. Check the version that you are using and update to latest version of the plugin by running the following commands:

```sh
ibmcloud plugin list
ibmcloud plugin update analytics-engine-v3
```
{: codeblock}
