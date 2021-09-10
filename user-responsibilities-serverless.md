---

copyright:
  years: 2019, 2021
lastupdated: "2021-09-08"

subcollection: analyticsengine

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
<!-- The title of your H1 should be Understanding your responsibilities with using _service-name_, where _service-name_ is the non-trademarked short version conref. -->

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.iae_full_notm}} serverless instances. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.iae_full_notm}} serverless instances. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

<!-- If you plan to list resource (see resources listed in each table in the platform shared responsibilities topic linked above) responsibility instead of individual tasks, you do not need to include rows for Hypervisor, Physical Servers and memory, Physical storage, Physical network and devices, and Facilities and data centers unless you need to indicate a 'Shared' or 'Customer' responsibility for one of the areas within those Resources. -->


## Incident and operations management
{: #incident-and-ops}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.


| Task | IBM responsibilities | Your responsibilities |
| ------ | ----------------------- | ---------------------- |
| {{site.data.keyword.iae_full_notm}} instance administration | - Provide infrastructure operating system (OS), version, and security updates.   \n- Clean up all instance resources.  \n-  Track hardware issues on running cluster.  | - Create an instance using the provided API, CLI or console tools.  \n- Delete a service instance using the provided API, CLI or console tools.   \n- Customize a service instance using the provided API or CLI.  \n- View or change the instance configuration using the provided API, CLI or console tools. |
| Application administration | - Monitor Spark application for any failures due to infrastructure provided by IBM. | - Run Spark applications on the cluster using the provided CLI or API.  \n- Tune the Spark instance for your application requirements using the provided CLI or API. |
| Observability | - Provide {{site.data.keyword.la_short}} to enable observability of your {{site.data.keyword.iae_full_notm}} service logs.  \n-  Provide integration with {{site.data.keyword.at_short}} to send {{site.data.keyword.iae_full_notm}} events for auditability. | - Set up {{site.data.keyword.at_short}} and send events to monitor the health of your {{site.data.keyword.iae_full_notm}} instances.  \n- Set up and send logs to {{site.data.keyword.la_short}}. |
{: row-headers}
{: caption="Table 1. Responsibilities for incident and operations" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Instance provisioning | - Order hardware (data plane in the IBM services account).  \n- Open the Spark cluster to the internet (data plane in the IBM Services account).  \n- Ensure network isolation of the Spark cluster nodes from other clusters (data plane in the IBM Services account).  \n- Patch the cluster hosts (data plane in the IBM Services account).   \n- Ensure safe erasure of data from removed node or deleted cluster nodes.   \n- Delete hardware (data plane in the IBM Services account) | - No change management responsibilities	|
{: row-headers}
{: caption="Table 2. Responsibilities for change management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Identity and access management
{: #iam-responsibilities}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task  | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Access control of the service instance through IAM | - Verify the user's permissions on the service instance before allowing access. | - Maintain responsibility for any service roles that you create for your instances.	|
{: row-headers}
{: caption="Table 3. Responsibilities for identity and access management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

| Task | IBM Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| General | - Maintain controls commensurate to various industry compliance standards.  \n- Monitor, isolate, and recover instances.  \n- Monitor and report the health of instances in the various interfaces.   \n- Secure cluster access through TLS/SSH (data plane in the IBM Services account).  \n- Integrate {{site.data.keyword.iae_full_notm}} with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). | -  Set up and maintain security and regulation compliance for the  {{site.data.keyword.iae_full_notm}} instances. |
{: row-headers}
{: caption="Table 4. Responsibilities for security and regulation compliance" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery}

<!-- Use this section description exactly as worded. -->
<!-- If there is a task that is the customer's responsibility and you have associated docs for how a customer completes that task, link to it from the Your responsibilities column. -->

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| General | - Restore or rebuild the provisioning environments in the affected regions.  \n- Restore existing Spark clusters, where possible. | - Track instance state.  \n- Provision new Spark instances in alternatively available regions.   \n- Ensure that the Spark instance is stateless by making sure that all data, metadata and applications reside outside of the cluster. This activity must be completed before disaster recovery can be initiated.  \n- Provision a new service instance in an alternatively available region if the current instances can't be accessed.  \n- Track instance state. |
{: row-headers}
{: caption="Table 5. Responsibilities for disaster recovery" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<!--
## {{site.data.keyword.iae_full_notm}} serverless instance responsibilities

Review the responsibilities that you share with IBM to manage your {{site.data.keyword.iae_full_notm}} serverless instances.

The following roles exist:
- **Responsible:** In this role, you do the work to complete the activity.
- **Accountable:** In this role, you approve the activity was successfully fulfilled.
- **Consulted:** In this role, are consulted to provide your expertise.
- **Informed:** In this role, you are notified of progress or completion of the activity.

The following table shows the roles and responsibilities that you share with IBM for key {{site.data.keyword.iae_full_notm}} processes.

| Area | Activity | Your responsibility | IBM responsibility |
|------|----------------|-------------------|------------- |
| Instance provisioning	| Order hardware (data plane in the IBM services account) |	None | Responsible and accountable |
| | Open the Spark cluster to the internet (data plane in the IBM Services account) | None	| Responsible and accountable |
| | Network isolation of Spark cluster nodes from other clusters (data plane in the IBM Services account)	| None	| Responsible and accountable |
| | Patch cluster hosts (data plane in the IBM Services account) | Informed | Responsible and accountable |
| | Safe erasure of data from removed node or deleted cluster nodes | None | Responsible and accountable |
| | Delete hardware (data plane in the IBM Services account) | None	| Responsible and accountable |
| Security | Secure cluster access through TLS/SSH (data plane in the IBM Services account) | None | Responsible and accountable |
| | Create service keys | Responsible and accountable	| Consulted |
| | Access control of the service instance through IAM | Responsible and accountable | Consulted |
| Instance administration	| Request to create a new service instance | Responsible and accountable | Consulted |
| | Request to delete a service instance | Responsible and accountable | Consulted |
| | Request to customize a service instance | Responsible and accountable | Consulted |
| | Request to get or view instance details | Responsible and accountable | Consulted |
| | View or change the instance configuration	| Responsible and accountable | Consulted |
| | Hardware issues on running cluster | None	| Responsible and accountable |
| | Continuous deployment of OS patches for mitigating security vulnerabilities |	None | Responsible and accountable |
| Application development or administration	| Run Spark applications on the cluster	| Responsible and accountable |	Consulted |
| | Tune the Spark instance	| Responsible and accountable	| Consulted |
| | View Spark application logs | Responsible and accountable | Consulted |
| | View Spark application history | Responsible and accountable | Consulted |
| | Externalize data in Cloud Object Storage and control access to the data | Responsible and accountable | Consulted |
| Disaster recovery for the provisioning systems | Restore or rebuild the provisioning environments in the affected regions |	None | Responsible and accountable |
| | Provision new Spark instances in alternatively available regions | Responsible and accountable |	Consulted |
| Disaster recovery for existing cluster operations |	Ensure that the Spark instance is stateless by making sure that all data, metadata and applications reside outside of the cluster. This activity must be completed before disaster recovery can be initiated. | Responsible and accountable | Consulted |
| | Provision a new service instance in an alternatively available region if  the current cluster can't be accessed. | Responsible and accountable | Consulted |
| | Restore the existing Spark cluster, where possible. |	Informed | Responsible |
-->
