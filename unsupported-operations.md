---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-22"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Unsupported operations
{: #unsupported-operations}

The following functionality is no longer supported in {{site.data.keyword.iae_full_notm}}.

## `AE 1.0` clusters: No instance provisioning and cluster resizing
{: #ae1.0-deprecation}

The `AE 1.0` software packages based on Hortonworks Data Platform (HDP) 2.6.2 are deprecated. You can no longer provision new instances of {{site.data.keyword.iae_full_notm}} with the `AE 1.0` software packages. Although you can still work on the `AE 1.0` clusters you have created, you can't resize those clusters and add additional nodes.

Although all existing `AE 1.0 ` clusters will only be deleted after September 30, 2019, you should stop using those clusters now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

## `AE 1.2` clusters: No cluster resizing on Hive LLAP clusters
{: #ae1.2-hive-llap}

Currently, you can't resize a cluster created using the `AE 1.2 Hive LLAP` software package. You need to plan your cluster size before you create the  cluster and specify the required number of nodes at the time you provision the {{site.data.keyword.iae_full_notm}} service instance.

## All `AE` clusters: Software packages installed only from the centOS repository
{: #ae-all-centos-repo}

The package-admin tool can only install software packages from the centOS repository.

For security reasons, you should use the `package-admin` tool to install, update, or remove operating system packages from the centOS repository.

## All AE cluster versions: OS packages lost after reboot

OS packages that are installed through the package-admin tool will not persist if the host machine is rebooted. These packages need to be installed again.  
