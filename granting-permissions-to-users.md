---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-26"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Granting permissions to users
{: #grant-permissions}

With an {{site.data.keyword.Bluemix_notm}} account, you have administrative privileges for your account, which enables you to perform all operations on an {{site.data.keyword.iae_full_notm}} service. However, when you onboard other users to your account, you need to manage their permissions so that they have the required privileges to operate service instances under your account.

Access to {{site.data.keyword.iae_full_notm}} resources requires certain access permissions at the resource group level, the platform level, and the service level. A user is given the desired level of access to a service instance only after the required roles at all of these levels were granted.

<table>
    <tr>
        <th>Operation</th>
        <th>Required IAM permissions</th>
    </tr>
    <tr>
        <td>Create or delete a service</td>
        <td>**Access type**: Resource group <br>
        **Role**: Editor or Administrator</td>
    </tr>
    <tr>
        <td>View the service dashboard (the cluster management UI)</td>
        <td>**Access type**: Service <br>
            **Platform role:** Viewer or above <br>
            **Service role**: Reader or above </td>
    </tr>
    <tr>
        <td>View cluster password in the cluster management UI</td>
        <td>**Access type**: Service <br>
            **Platform role**: Viewer or above <br>
            **Service role**: Writer or Manager </td>
    </tr>
    <tr>
        <td>Resize a cluster by using the cluster management UI</td>
        <td>**Access type**: Service <br>
            **Platform role:** Viewer or above <br>
            **Service role:** Writer or Manager </td>
    </tr>
    <tr>
        <td>Create service keys by using the IBM Cloud UI <br>
        **Note**: A service key created for a Reader role does not reveal the cluster password. </td>
        <td>**Access type**: Service <br>
            **Platform role**: Operator or above </td>
    </tr>
    <tr>
        <td>View service keys by using the IBM Cloud UI</td>
        <td>**Access type**: Service <br>
            **Platform role**: Viewer or above </td>
    </tr>
    <tr>
        <td>Invoke the cluster management REST API to view cluster details</td>
        <td>**Access type**: Service <br>
            **Service role**: Reader, Writer or Manager <br>
            **Note**: When the API is invoked with service role as Reader, the response of the API does not reveal the cluster password. </td>
    </tr>
    <tr>
        <td>Invoke cluster management REST API to view customization request details or list of customization requests</td>
        <td>**Access type**: Service <br>
            **Service role**: Reader, Writer or Manager</td>
    </tr>
    <tr>
        <td>Invoke cluster management REST API to resize cluster</td>
        <td>**Access type**: Service <br>
            **Service role**: Writer or Manager </td>
    </tr>
    <tr>
        <td>Invoke cluster management REST API to add an adhoc customization request</td>
        <td>**Access type**: Service <br>
            **Service role**: Writer or Manager </td>
    </tr>
    <tr>
        <td>Invoke cluster management REST API to reset cluster password</td>
        <td>**Access type**: Service <br>
            **Service role**: Manager </td>
    </tr>
    <caption style="caption-side:bottom;">Table 1. Required IAM privileges for user onboarding</caption>
    </table>



To onboard new users to your account:

1.	Log on to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://{DomainName}).

2.	Click **Manage -> Account -> Users**.

3.	In the User management page, click **Invite users**.

4.	Enter the IBMid of the user you want to invite.

5.	Under the Access section, expand **Services** and select the following values.

 To assign access at a resource group level:

	a.	Assign access to: Select **Resource group**.

	b.	Resource group: Choose a resource group to which to grant access.

	c.	Assign access to a resource group: Select the level of access you want to provide.

  To assign access at a Resource level:

   a. Assign Access to: Select **Resource**.

   b. Services: **{{site.data.keyword.iae_short}}**.

   c. Region: Choose the region, for example **US-South** depending on where your resource resides.

   d. Service Instance: Choose the service instance that you want to grant access to.   

   e. Select roles: Select the levels of access you want to provide.
