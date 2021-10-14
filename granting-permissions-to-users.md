---

copyright:
  years: 2017, 2021
lastupdated: "2021-01-07"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Granting permissions to users
{: #grant-permissions}

With an {{site.data.keyword.Bluemix_notm}} account, you have administrative privileges for your account, which enables you to perform all operations on an {{site.data.keyword.iae_full_notm}} service. However, when you onboard other users to your account, you need to manage their permissions so that they have the required privileges to operate service instances under your account.

Access to {{site.data.keyword.iae_full_notm}} resources requires certain access permissions at the resource group level, the platform level, and the service level. A user is given the desired level of access to a service instance only after the required roles at all of these levels were granted.

The following table lists the required IAM privileges for user onboarding.

| Operations             | Required IAM permissions      |
|------------------------|-------------------------------|
|Create or delete a service |**Access type**: Resource group \n **Role**: Editor or Administrator |
|View the service dashboard (the cluster management UI) |**Access type**: Service **Platform role:** Viewer or above \n **Service role**: Reader or above    |  
| View cluster password in the cluster management UI|**Access type**: Service \n  **Platform role**: Viewer or above \n **Service role**: Writer or Manager|
|Resize a cluster by using the cluster management UI|**Access type**: Service \n  **Platform role:** Viewer or above \n **Service role:** Writer or Manager|
|Create service keys by using the IBM Cloud UI \n **Note**: A service key created for a Reader role does not reveal the cluster password.|**Access type**: Service \n **Platform role**: Operator or above |
|View service keys by using the IBM Cloud UI|**Access type**: Service \n  **Platform role**: Viewer or above|
|Invoke the cluster management REST API to view cluster details|**Access type**: Service \n **Service role**: Reader, Writer or Manager \n **Note**: When the API is invoked with service role as Reader, the response of the API does not reveal the cluster password.|
|Invoke cluster management REST API to view customization request details or list of customization requests|**Access type**: Service \n **Service role**: Reader, Writer or Manager|
|Invoke cluster management REST API to resize cluster|**Access type**: Service \n **Service role**: Writer or Manager|
|Invoke cluster management REST API to add an adhoc customization request|**Access type**: Service \n **Service role**: Writer or Manager|
|Invoke cluster management REST API to reset cluster password|**Access type**: Service \n **Service role**: Manager|
|Invoke cluster management REST API to create or delete log configuration|**Access type**: Service \n **Service role**: Writer or Manager|
|Invoke cluster management REST API to retrieve log configuration details|**Access type**: Service \n **Service role**: Reader|
|Update cluster private endpoint allowlist|**Access type**: Service \n **Service role**: Writer or Manager |


To onboard new users to your account:

1. Log on to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://{DomainName}){: external}.
1. Click **Manage -> Account -> Users**.
1. In the User management page, click **Invite users**.
1. Enter the IBMid of the user you want to invite.
1. Under the Access section, expand **Services** and select the following values.

    - To assign access at a resource group level:

        1. Assign access to: Select **Resource group**.
        1. Resource group: Choose a resource group to which to grant access.
        1. Assign access to a resource group: Select the level of access you want to provide.
    - To assign access at a Resource level:

        1. Assign Access to: Select **Resource**.
        1. Services: **{{site.data.keyword.iae_short}}**.
        1. Region: Choose the region, for example **US-South** depending on where your resource resides.
        1. Service Instance: Choose the service instance that you want to grant access to.
        1. Select roles: Select the levels of access you want to provide.
