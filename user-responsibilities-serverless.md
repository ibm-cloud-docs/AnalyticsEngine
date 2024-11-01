---

copyright:
  years: 2019, 2024
lastupdated: "2024-03-05"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Understanding your responsibilities when using {{site.data.keyword.iae_full_notm}} serverless instances
{: #responsibilities-serverless}



Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.iae_full_notm}} serverless instances. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.iae_full_notm}} serverless instances. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).




## Incident and operations management
{: #incident-and-ops}




Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.


| Task | IBM responsibilities | Your responsibilities |
| ------ | ----------------------- | ---------------------- |
| {{site.data.keyword.iae_full_notm}} instance administration | - Provide infrastructure operating system (OS), version, and security updates.   \n- Clean up all instance resources.  \n-  Track hardware issues on running cluster.  | - Create an instance using the provided API, CLI or console tools.  \n- Delete a service instance using the provided API, CLI or console tools.   \n- Customize a service instance using the provided API or CLI.  \n- View or change the instance configuration using the provided API, CLI or console tools. |
| Application administration | - Monitor Spark application for any failures due to infrastructure provided by IBM. | - Run Spark applications on the cluster using the provided CLI or API.  \n- Tune the Spark instance for your application requirements using the provided CLI or API. |
| Observability | - Provide {{site.data.keyword.la_short}} to enable observability of your {{site.data.keyword.iae_full_notm}} service logs.  \n-  Provide integration with {{site.data.keyword.at_short}} to send {{site.data.keyword.iae_full_notm}} events for auditability. | - Set up {{site.data.keyword.at_short}} and send events to monitor the health of your {{site.data.keyword.iae_full_notm}} instances.  \n- Set up and send logs to {{site.data.keyword.la_short}}. |
{: caption="Responsibilities for incident and operations" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management}




Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Instance provisioning | - Order hardware (data plane in the IBM services account).  \n- Open the Spark cluster to the internet (data plane in the IBM Services account).  \n- Ensure network isolation of the Spark cluster nodes from other clusters (data plane in the IBM Services account).  \n- Patch the cluster hosts (data plane in the IBM Services account).   \n- Ensure safe erasure of data from removed node or deleted cluster nodes.   \n- Delete hardware (data plane in the IBM Services account) | - No change management responsibilities	|
{: caption="Responsibilities for change management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Identity and access management
{: #iam-responsibilities}




Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task  | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Access control of the service instance through IAM | - Verify the user's permissions on the service instance before allowing access. | - Maintain responsibility for any service roles that you create for your instances.	|
{: caption="Responsibilities for identity and access management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-regulation}




Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| General | - Maintain controls commensurate to various industry compliance standards.  \n- Monitor, isolate, and recover instances.  \n- Monitor and report the health of instances in the various interfaces.   \n- Secure cluster access through TLS (data plane in the IBM Services account).  \n- Integrate {{site.data.keyword.iae_full_notm}} with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). | -  Set up and maintain security and regulation compliance for the  {{site.data.keyword.iae_full_notm}} instances. |
{: caption="Responsibilities for security and regulation compliance" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## High availability and Disaster recovery
{: #disaster-recovery}




High availability (HA) is a core discipline in an IT infrastructure to keep your apps up and running, even after a partial or full site failure. The main purpose of high availability is to eliminate potential points of failures in an IT infrastructure.

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| High availability | - IBM ensures that the control plane is deployed on multi zone regions. When a zone becomes unavailable in a multi zone region, the workloads are automatically scheduled on the remaining available zones.  \n- Maintain service replicas to ensure service availability on Pod failure. | No action required|
| General | - Restore or rebuild the provisioning environments in the affected regions.  \n- Rebuild the existing Spark instance, where possible. | - Track instance state and application state.  \n- Provision a new service instance and re-submit the application in an alternatively available region if the current instances can't be accessed.  \n- Create backup for all Spark instance configuration data and validate the information.  \n- Make sure that all data, metadata and applications reside outside of the cluster. This activity must be completed before disaster recovery can be initiated.  |
{: caption="Responsibilities for disaster recovery" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Locations
{: #loc}

* Frankfurt
* Dallas
