---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-02"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Activity Tracker events
{: #at-tracker}

Use the {{site.data.keyword.Bluemix_short}} service to track how users and applications interact with {{site.data.keyword.iae_full_notm}}.

The {{site.data.keyword.Bluemix_short}} Activity Tracker service records user-initiated activities that change the state of a service in {{site.data.keyword.Bluemix_short}}.

For more information, see the [Activity Tracker documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov#activity_tracker_ov){: new_window}.

The following table lists the actions that generate an event:

## List of events
{: #events}

<table>
    <tr>
        <th>Action</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>ibmanalyticsengine.cluster-details.read</td>
        <td>Retrieve the details of the Analytics Engine cluster</td>
    </tr>
    <tr>
        <td>ibmanalyticsengine.cluster-state.read</td>
        <td>Retrieve the state of the Analytics Engine cluster</td>
    </tr>
   <tr>
        <td>ibmanalyticsengine.cluster-customization-details.read</td>
        <td>Retrieve the details of the customizations done on the Analytics Engine cluster</td>
    </tr>
    <tr>
        <td>ibmanalyticsengine.cluster-customizations-history.read</td>
        <td>Retrieve the history of customization actions done on the Analytics Engine cluster</td>
    </tr>
    <tr>
        <td>ibmanalyticsengine.cluster-customization.create</td>
        <td>Create a customization on the Analytics Engine cluster</td>
    </tr>
     <tr>
        <td>ibmanalyticsengine.cluster.resize</td>
        <td>Add more nodes to the Analytics Engine cluster</td>
    </tr>
     <tr>
        <td>ibmanalyticsengine.cluster-password.reset</td>
        <td>Reset the Analytics Engine cluster credentials</td>
    </tr>
     <tr>
       <td>ibmanalyticsengine.cluster-log-config.create</td>
       <td>Create a log aggregation configuration</td>
    </tr>
     <tr>
      <td>ibmanalyticsengine.cluster-log-config.read</td>
      <td>Retrieve the log aggregation configuration details</td>
    </tr>
     <tr>
       <td>ibmanalyticsengine.cluster-log-config.delete</td>
       <td>Delete a log aggregation configuration</td>
    </tr>
    <caption style="caption-side:bottom;">Table 1. Actions that generate {{site.data.keyword.iae_full_notm}} events</caption>
</table>

## Where to view the events
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.cloud_notm}} region where the events are generated.

For example, when you perform an action in  {{site.data.keyword.iae_full_notm}}, an {{site.data.keyword.cloudaccesstrailshort}} event is generated. These events are automatically forwarded to the {{site.data.keyword.cloudaccesstrailshort}} service closest to the  region where the {{site.data.keyword.iae_full_notm}} service is provisioned.

To monitor activity, you must provision the {{site.data.keyword.cloudaccesstrailshort}} service in a space that is available in a region closest to the region where your   {{site.data.keyword.iae_full_notm}} service is provisioned. Then, you can view events through the account view in the {{site.data.keyword.cloudaccesstrailshort}} UI if you have a lite plan of {{site.data.keyword.cloudaccesstrailshort}}, and through Kibana if you have a premium plan.
