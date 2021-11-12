---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-13"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Using the Node.js SDK
{: #using-node-js-serverless}

The {{site.data.keyword.iae_full_notm}} Node.js SDK allows you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API for serverless instances.

## Installing the SDK
{: #node-install}

You can install the {{site.data.keyword.iae_full_notm}} Node.js SDK using the Node package manager (npm).

Type the following command into a command line:
```javascript
npm install iaesdk
```
{: codeblock}

You can find the source code in GitHub. See [ibm-iae-node-sdk](https://github.com/ibm/ibm-iae-node-sdk/){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

To run the Node.js SDK, you need Node 4.x+. You need to provide the service endpoints and the API key when you create a  {{site.data.keyword.iae_full_notm}} service resource or a low-level client.

The service instance ID is also referred to as a instance GUID. You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless){: external}.

## Code samples using `iaesdk`
{: #code-samples-node-js}

Getting started with the Node.js SDK after you have installed it, involves sourcing credentials to the {{site.data.keyword.iae_full_notm}} service, invoking the service and then issuing different cluster commands as shown in the following sample code snippets.

The code samples show how to:

- Authenticate to the {{site.data.keyword.iae_full_notm}} service and create a service client:
    ```javascript
    const IbmAnalyticsEngineApiV3 = require('iaesdk/ibm-analytics-engine-api/v3');
    const { IamAuthenticator } = require('iaesdk/auth');

    const IAM_API_KEY = "{apikey}" // eg "W00YiRnLW4a3fTjMB-odB-2ySfTrFBIQQWanc--P3byk"
    const IAE_ENDPOINT_URL = "{url}" // Current list available at https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints

    // Create an IAM authenticator.
    const authenticator = new IamAuthenticator({
      apikey: IAM_API_KEY,
      });

    // Construct the service client.
    const ibmAnalyticsEngineApiService = new IbmAnalyticsEngineApiV3({
      authenticator,
      serviceUrl: IAE_ENDPOINT_URL,
    });
    ```
    {: codeblock}

    Key values:

    - `serviceUrl`: public endpoint to the  {{site.data.keyword.iae_full_notm}} instance. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints){: external}.
    - `apikey`: API key generated when creating the service credentials. Write access is required for creation and deletion tasks.

- Retrieve the details of a single instance:
    ```javascript
    getInstance(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    const params = {
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
    };
    ibmAnalyticsEngineApiService.getInstance(params)
    .then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}

- Deploy a Spark application on a given serverless Spark instance:
    ```javascript
    createApplication(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    // ApplicationRequestApplicationDetails
    const applicationRequestApplicationDetailsModel = {
      application: '/opt/ibm/spark/examples/src/main/python/wordcount.py',
      arguments: ['/opt/ibm/spark/examples/src/main/resources/people.txt'],
    };

    ibmAnalyticsEngineApiService.createApplication({
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
      applicationDetails: applicationRequestApplicationDetailsModel,
    }).then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}

- Retrieve all Spark applications run on a given instance:
    ```javascript
    listApplications(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    ibmAnalyticsEngineApiService.listApplications({
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
    }).then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}

- Retrieve the details of a given Spark application:
    ```javascript
    getApplication(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    ibmAnalyticsEngineApiService.getApplication({
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
      applicationId: 'db933645-0b68-4dcb-80d8-7b71a6c8e542',
    }).then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}

- Return the state of the application indentified by the `app_id`identifier:
    ```javascript
    getApplicationState(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    ibmAnalyticsEngineApiService.getApplicationState({
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
      applicationId: 'db933645-0b68-4dcb-80d8-7b71a6c8e542',
    }).then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}

- Stop a running application identified by the `app_id` identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
    ```javascript
    deleteApplication(params)
    ```
    {: codeblock}

    Example request:
    ```javascript
    ibmAnalyticsEngineApiService.deleteApplication({
      instanceId: 'e64c907a-e82f-46fd-addc-ccfafbd28b09',
      applicationId: 'db933645-0b68-4dcb-80d8-7b71a6c8e542',
    }).then((res) => {
      console.log(JSON.stringify(res.result, null, 2));
    }).catch((err) => {
      console.warn(err);
    });
    ```
    {: codeblock}


