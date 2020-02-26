---

copyright:
  years: 2017, 20120
lastupdated: "2020-02-25"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Enabling Spark jobs encryption
{: #spark-encryption}

All service endpoints (including those of Spark interfaces) to the cluster are SSL encrypted. However the internal communication on the private network between the worker nodes of the cluster is not encrypted by default.

You can enable encryption in the private network explicitly by performing the following step:

1.  Provision a cluster and provide the following advanced custom configuration options. These options enable encrypting data that is passed between the nodes when a Spark job runs by making use of inbuilt certificates that are already installed on the cluster.
```
{
      "spark2-defaults": {
          "spark.authenticate": "true",
          "spark.authenticate.enableSaslEncryption": "true",
          "spark.ssl.enabled":"true"
      },
      "spark2-thrift-sparkconf": {
          "spark.authenticate": "true"
      },
      "yarn-site": {
          "spark.authenticate": "true"
      }
}
```
For more information on how to add advanced configuration options through Ambari, see [Advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options).
1. After the cluster is created, run an adhoc customization script to make changes to the Knox topology to enable the Spark History UI.
```
"actions": [{
		"name": "action1 http",
		"type":"bootstrap",
		"script": {
			"source_type": "http",
			"script_path": "https://raw.githubusercontent.com/IBM-Cloud/IBM-Analytics-Engine/master/customization-examples/enable-spark-encryption.sh"
		},
 		"script_params": ["<CHANGEME_clsadminpassword_CHANGEME>"]

	}]
```
