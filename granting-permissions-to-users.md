---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Granting permissions to users

With an IBM {{site.data.keyword.Bluemix_notm}} account, you have administrative privileges in the organization or space under your account, which enables you to perform all operations on an {{site.data.keyword.iae_full_notm}} service. However, when you onboard other users to your account, you need to manage their permissions so that they have the required privileges to operate service instances under your account.

In {{site.data.keyword.iae_full_notm}}s service, access to cluster life cycle management operations is governed by the following permission levels:

| Operation | Required {{site.data.keyword.Bluemix_notm}} permissions | Required IAM permissions |
|-----------|------------------------------|--------------------------|
| Create or delete a service | Developer role to the {{site.data.keyword.Bluemix_notm}} space | Editor and above to your Resource Group |
| View the service dashboard (Cluster Management UI) | Developer role to the {{site.data.keyword.Bluemix_notm}} space | Viewer and above to your Resource Group and Reader, or above to the service instance |
| Resize a cluster by using the Cluster Management UI | Developer role to the {{site.data.keyword.Bluemix_notm}} space | Editor and above to the service instance |
| Generate service keys by using the CF CLI or the {{site.data.keyword.Bluemix_notm}} UI | Developer role to the {{site.data.keyword.Bluemix_notm}} space | Editor or above to the service instance |
| Invoke  GET REST APIs on `https:// api.dataplatform.ibm.com/v2/analytics_engines` | NA | Reader and above |
| Invoke POST  REST APIs on `https:// api.dataplatform.ibm.com/v2/analytics_engines`| NA | Writer and above |

To onboard new users to your account:

1.	Log on to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://console.bluemix.net).

2.	Click **Manage -> Account -> Users**.

3.	In the User management page, click **Invite users**.

4.	Enter the IBMid of the user you want to invite.

5.	Under the Access section, expand **Services** and select the following values.

 To assign access at a Resource Group level:

	a.	Assign access to: Select **Resource Group**.

	b.	Resource Group: Choose a resource group to grant access to.

	c.	Assign access to a resource group: Select the level of access you want to provide.

  To assign access at a Resource level:

   a. Assign Access to: Select **Resource**.

   b. Services: **{{site.data.keyword.iae_short}}**.

   c. Region: Choose **US-South** or **United Kingdom** depending on where your resource resides.

   d. Service Instance: Choose the service instance that you want to grant access to.   

   e. Select roles: Select the levels of access you want to provide.

6.	Expand **Cloud Foundry access** and select the organization that you want to give the user access to.

	a. Select a role at the organization level for your user.

	b.	Choose a region.

	c.	Select the space that you want to grant the user access to.

	e.	Select the role that you want to assign to the user. To view the service dashboard and perform tasks such as generating service keys you must assign Developer role.
