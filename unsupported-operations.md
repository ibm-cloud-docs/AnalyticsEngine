---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}


# Unsupported operations
{: #unsupported-operations}

The following functionality is not supported in {{site.data.keyword.iae_full_notm}}.

## `AE 1.1` clusters: No instance provisioning and cluster resizing
{: #ae1.1-deprecation}

The `AE 1.1` software packages based on Hortonworks Data Platform (HDP) 2.6.5 are deprecated. You can no longer provision new instances of {{site.data.keyword.iae_full_notm}} with the `AE 1.1` software packages. Although you can still work on the `AE 1.1` clusters you have created, you can't resize those clusters and add additional nodes.

Although all existing `AE 1.1` clusters will only be deleted after December 31, 2019, you should stop using those clusters now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices){: new_window}.

## `AE 1.0` clusters: No instance provisioning and cluster resizing
{: #ae1.0-deprecation}

The `AE 1.0` software packages based on Hortonworks Data Platform (HDP) 2.6.2 are deprecated. You can no longer provision new instances of {{site.data.keyword.iae_full_notm}} with the `AE 1.0` software packages. Although you can still work on the `AE 1.0` clusters you have created, you can't resize those clusters and add additional nodes.

Although all existing `AE 1.0 ` clusters will only be deleted after September 30, 2019, you should stop using those clusters now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices){: new_window}.

## `AE 1.2` clusters: No cluster resizing on Hive LLAP clusters
{: #ae1.2-hive-llap}

Currently, you can't resize a cluster created using the `AE 1.2 Hive LLAP` software package. You need to plan your cluster size before you create the  cluster and specify the required number of nodes at the time you provision the {{site.data.keyword.iae_full_notm}} service instance.

## All `AE` clusters: OS packages can't be installed from a non standard CentOS repository
{: #ae-all-centos-repo}

The package-admin tool can only install software packages from the centOS repository.

For security reasons, you should use the `package-admin` tool to install, update, or remove operating system packages from the centOS repository.

## All `AE` cluster versions: OS packages lost after reboot
{: #os-oackages-lost}

OS packages that are installed through the package-admin tool are not persisted if the host machine is rebooted. These packages need to be installed again.  

## `AE 1.2` clusters: Hive View not supported
{: #hive-view-not-supported}

Hive View has been removed from the underlying platform in `AE 1.2`. You can use any other JDBC UI based client such as SQuirrel SQL or Eclipse Data Source Explorer as an alternative.

## `AE 1.2`cluster: Hive and Spark do not share the same metastore
{: #hive-spark-not-same-metastore}

From AE 1.2 onwards, Hive and Spark do not share the same metastore. This is by design in the underlying engine.

To override this behavior:
1. In the Ambari UI, navigate to **Ambari > Spark2 > Config > spark2-hive-site-override**.
1. Set `"metastore.catalog.default" : "hive"`.

Refer to the following links to understand the reasoning and repercussions for the change:
- [Hive Warehouse Connector for accessing Apache Spark data](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.4/integrating-hive/content/hive_hivewarehouseconnector_for_handling_apache_spark_data.html){: external}
- [Using the Hive Warehouse Connector with Spark](https://docs.cloudera.com/HDPDocuments/HDP3/HDP-3.1.4/developing-spark-applications/content/using_spark_hive_warehouse_connector.html){: external}

## AE 1.2 clusters: PostgreSQL bootstrap customization and advanced options not supported

The only way to configure a cluster to work with PostgreSQL is either through the UI or by using adhoc customization. See [Using an adhoc PostgreSQL customization script](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#configuring-a-cluster-to-work-with-postgresql){: new_window}.

Configuring the cluster at the time the cluster is created by using bootstrap customization is not supported because it requires the PostgreSQL certificate to be present on the `mn002` node before the Hive service is started. However the bootstrap script is only executed after the cluster was created. See [Advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options){: new_window}. There is no way to download the certificate prior to cluster creation.
