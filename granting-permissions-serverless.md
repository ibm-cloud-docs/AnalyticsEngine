---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-24"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:note: .note}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Granting permissions to users
{: #grant-permissions-serverless}

With an {{site.data.keyword.Bluemix_notm}} account, you have administrative privileges for your account, which enables you to perform all operations on {{site.data.keyword.iae_full_notm}} service instances. However, when you onboard other users to your account, you need to manage their permissions so that they have the required privileges to use {{site.data.keyword.iae_full_notm}} service instances under your account.

Access to {{site.data.keyword.iae_full_notm}} service instances for users in your account is controlled by {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.iae_full_notm}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions a user can perform within the context of the service or instance. Permitted actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

- Access policies enable access to be granted at different levels. You can see which access policies are set for you in the [{{site.data.keyword.cloud}} catalog](https://cloud.ibm.com/catalog){: external} console.

    1. Go to [Access IAM users](https://cloud.ibm.com/iam/users){: external}.
    1. Click your name in the user table.
    1. Click the **Access policies** tab to see your access policies.
- Roles define the actions that a user or service ID can run. There are different types of roles in the {{site.data.keyword.cloud_notm}}:

    - *Resource group* roles. When you create an {{site.data.keyword.iae_full_notm}} service instance, you assign the service to a resource group. This resource group helps you to organize your account resources for access control. Users with resource group roles can create or delete service instances.
    - *Service access* roles. Users with service access roles can be assigned varying levels of permission for calling the service's API.

Access to {{site.data.keyword.iae_full_notm}} resources requires certain access permissions at the resource group level and the service level. A user is given the desired level of access to a service instance only after the required roles at  these levels were granted.

## {{site.data.keyword.cloud_notm}} platform roles
{: #platform}

Platform management roles enable users to perform tasks on service resources at the platform level (in your resource group), for example, create or delete instances, and assign other users access to a service instance.

Use the following table to identify the platform role that you can grant a user in the {{site.data.keyword.cloud_notm}} to run any of the following platform actions:

| Platform actions   | Administrator   | Editor | Operator | Viewer  |
|--------------------------|:--------------------------:|:-------:|:--------:|:------:|
| Provision a service instance. | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |          |        |
| Delete a service instance. | ![the confirm icon](images/confirm.png) |  ![the confirm icon](images/confirm.png)      |          |        |
{: caption="Platform roles and actions" caption-side="top"}
{: #iam-table-1}
{: row-headers}


## {{site.data.keyword.cloud_notm}} service roles
{: #service}

Use the following table to identify the service roles that you can grant a user to run any of the following service actions:

| Actions  | Manager               | Writer         | Reader |
|----------|-----------------------|----------------|--------|
|Invoke the instance management REST API to view instance details| ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|Invoke the instance management REST API to set instance home| ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |  |
|Invoke application management REST API to submit a Spark application | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | |
|Invoke application management REST API to stop a submitted application | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | |
|Invoke application management REST API to view all submitted Spark applications | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|Invoke application management REST API to view a specific Spark application, by using the application ID | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png)|
|Invoke application management REST API to retrieve the state  of a Spark application | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|Invoke instance management REST API to enable or disable logging | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |  |
|Invoke instance management REST API to view logging configuration | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|Invoke instance management REST API to start or stop Spark history server | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |  |
|Invoke instance management REST API to view Spark history server state | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|View Spark History UI | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
|Invoke Spark History REST API | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) | ![the confirm icon](images/confirm.png) |
{: caption="IAM service roles and actions" caption-side="top"}
{: #iam-table-2}
{: row-headers}


To onboard new users to your account:

1. Log on to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://{DomainName}){: external}.
1. Click **Manage -> Access (IAM)** from the **Manage** menu on the {{site.data.keyword.Bluemix_notm}} console.
1. On the Manage access and users page, click **Invite users**.
1. Enter the email addresses of the users you want to invite.
1. Expand the **Add users to access groups** section and add the users to an access group. You can create access groups from here if needed.
1. Assign those users access to your {{site.data.keyword.iae_full_notm}} service instance by expanding the section **Assign users additional access** and  selecting **IAM services**.

    1. Select **{{site.data.keyword.iae_short}}** from the list of access types.
    1. Select **Services based on attributes** and choose the {{site.data.keyword.iae_short}} service instance that you want to grant access to in your resource group and at your location.
    1. Select the level of access you want to enable by choosing the appropriate roles.
