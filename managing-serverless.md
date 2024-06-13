---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-25"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Managing serverless instances using the IBM Cloud console
{: #manage-serverless-console}

You can manage your severless instance by:

- Changing configuration settings, for example, to include library add-ons or to configure instance home after you created the instance.
- Monitoring the status of submitted applications and kernels created in the instance.    

## Console configuration tab
{: #config-tab}

You can view and edit the current configuration settings for your {{site.data.keyword.iae_full_notm}} serverless instance from the {{site.data.keyword.Bluemix_short}} Resource list.

1. Access the [{{site.data.keyword.Bluemix_short}} Resource list](https://test.cloud.ibm.com/resources).
1. Click **Services and software**, find your  {{site.data.keyword.iae_full_notm}} serverless instance and click the instance to see the details.
1. Click **Manage > Configuration** to view:

    - The runtime. Currently, you can only select the `Default Spark` runtime which includes the geospatial, data skipping and Parquet encryption packages.
    - The `instance home` volume to add an `instance home` or change the access credentials of an existing `instance home`

        - You can set `instance home` after you created your {{site.data.keyword.iae_full_notm}} serverless instance. `Instance home` must be associated with an {{site.data.keyword.cos_full_notm}} instance. You can choose an instance:

          - In your account by selecting it from the list
          - From another account. For this instance, you need to enter:

              - The GUID of the {{site.data.keyword.cos_full_notm}} instance
              - The endpoint
              - The region
              - The HMAC access and secret key
        - You can change the access credentials of an existing `instance home` volume. For this instance, you need to enter:

             - The new HMAC access and secret key   

         For details on how to access Object Storage, see [Using IBM Object Storage as the instance home volume](/docs/AnalyticsEngine?topic=AnalyticsEngine-cos-serverless).      
    - The `default Spark configuration` options to override configuration settings.

      For a list of the default Spark configurations set for serverless instances, see [Default Spark configurations](/docs/AnalyticsEngine?topic=AnalyticsEngine-serverless-architecture-concepts#default-spark-config).



## Console applications tab
{: #apps-tab}

You can monitor the status of submitted applications in your {{site.data.keyword.iae_full_notm}} serverless instance from the {{site.data.keyword.Bluemix_short}} Resource list.

1. Access your [{{site.data.keyword.Bluemix_short}} Resource list](https://test.cloud.ibm.com/resources).
1. Click **Services and software**, find the   {{site.data.keyword.iae_full_notm}} serverless instance and click the instance to see its details.
1. Click **Manage > Applications** to list the submitted applications. You can:

    - Click the Filter icon on the action bar of the result list to narrow your search results to find applications by their status.
    - Click the Settings icon to customize the result list by adjusting the row height or the display columns.
    - Refresh the list

    By clicking the arrow to the left of an application ID in the result list, you are shown additional properties that are not included in any list columns, for example, the ID assigned to the application by Spark.  

    By clicking the Action icon to the right of a selected application, you can:

      - Stop the application



