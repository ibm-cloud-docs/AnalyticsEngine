---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-28"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Your responsibilities by using {{site.data.keyword.iae_full_notm}}
{: #user-responsibilities}

Learn about cluster management responsibilities and terms and conditions that you have when you use the {{site.data.keyword.iae_full_notm}} service.

## {{site.data.keyword.iae_full_notm}} service responsibilities

Review the responsibilities that you share with IBM to manage your {{site.data.keyword.iae_full_notm}} cluster.

The following roles exist:
- **Responsible:** In this role, you do the work to complete the activity.
- **Accountable:** In this role, you approve the activity was successfully fulfilled.
- **Consulted:** In this role, are consulted to provide your expertise.
- **Informed:** In this role, you are notified of progress or completion of the activity.

The following table shows the roles and responsibilities that you share with IBM for key {{site.data.keyword.iae_full_notm}} processes.

| Area | Activity | Your responsibility | IBM responsibility |
|------|----------------|-------------------|------------- |
|Cluster provisioning | Order hardware (data plane in IBM services account)| None | Responsible and accountable|
| | Open the cluster to the internet (data plane in IBM Services account)| None |Responsible and accountable|
| | Network isolation of cluster nodes from other clusters (data plane in IBM Services account)| None |Responsible and accountable|
| |Patch cluster hosts (data plane in IBM Services account)| Informed |Responsible and accountable|
| |Install HDP and bring up all selected services| Informed |Responsible and accountable|
| |Safe erasure of data from removed node or deleted cluster nodes | None |Responsible and accountable|
| |Delete hardware (data plane in IBM Services account)| None |Responsible and accountable|
|Security |Secure cluster access through TLS/SSH (data plane in IBM Services account)| None |Responsible and accountable|
| |Create service keys| Responsible and accountable |Consulted|
| |Reset user password of cluster| Responsible and accountable| Consulted|
| |SSH to cluster nodes |Responsible and accountable| Consulted|
| |Access control of the service instance through IAM| Responsible and accountable| Consulted |
|Cluster administration | Request to create a new cluster (service instance)| Responsible and accountable| Consulted |
| |Request to delete a cluster| Responsible and accountable| Consulted |
| |Request to add nodes to a cluster| Responsible and accountable| Consulted |
| |Request to customize a cluster (bootstrap)| Responsible and accountable| Consulted |
| |Request to customize a cluster (adhoc)| Responsible and accountable| Consulted |
| |Request to get or view cluster details| Responsible and accountable| Consulted |
| |View or change the cluster configuration |Responsible and accountable| Consulted |
| |Start or stop HDP component services (like Spark, JNBG, or Hive) |Responsible and accountable| Consulted |
| |Start or stop the Ambari server, local LDAP, or guest OS| None  |Responsible and accountable|
|Application development or administration | Run jobs on the cluster | Responsible and accountable| Consulted |
| | Tune the cluster| Responsible and accountable| Consulted |
| | View job logs| Responsible and accountable| Consulted |
| | View job history| Responsible and accountable| Consulted |
| | Externalized Hive metadata |Responsible and accountable| Consulted |
| | Externalize data in Cloud Object Storage and control access to the data |Responsible and accountable| Consulted |
