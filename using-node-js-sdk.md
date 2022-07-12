---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-14"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Using the Node.js SDK
{: #using-node-js}

The {{site.data.keyword.iae_full_notm}} Node.js SDK allows you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.

## Installing the SDK
{: #node-install}

You can install the {{site.data.keyword.iae_full_notm}} Node.js SDK using the Node package manager (npm).

Type the following command into a command line:
```javascript
npm install iaesdk
```

You can find the source code in GitHub. See [ibm-iae-node-sdk](https://github.com/ibm/ibm-iae-node-sdk/){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

To run the Node.js SDK, you need Node 4.x+. You need to provide the service endpoints and the API key when you create a  {{site.data.keyword.iae_full_notm}} service resource or a low-level client.

The service instance ID is also referred to as a instance GUID. You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints){: external}.

## Code samples using `iaesdk`
{: #code-samples-node-js}

Getting started with the Node.js SDK after you have installed it, involves sourcing credentials to the {{site.data.keyword.iae_full_notm}} service, invoking the service and then issuing different cluster commands as shown in the following sample code snippets.

The code samples show how to:

- Authenticate to the {{site.data.keyword.iae_full_notm}} service and create a service client:
    ```javascript
    const IbmAnalyticsEngineApiV2 = require('iaesdk/ibm-analytics-engine-api/v2');
    const { IamAuthenticator } = require('iaesdk/auth');

    const IAM_API_KEY = "<api-key>" // eg "W00YiRnLW4a3fTjMB-odB-2ySfTrFBIQQWanc--P3byk"
    const IAE_ENDPOINT_URL = "<endpoint>" // Current list available at https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints

    // Create an IAM authenticator.
    const authenticator = new IamAuthenticator({
      apikey: IAM_API_KEY,
      });

    // Construct the service client.
    const IbmAnalyticsEngineServiceClient = new IbmAnalyticsEngineApiV2({
      authenticator,
      serviceUrl: IAE_ENDPOINT_URL,
    });
    ```
    {: codeblock}

    Key values:

    - `<endpoint>`: public endpoint to the  {{site.data.keyword.iae_full_notm}} instance. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints){: external}.
    - `<api-key>`: API key generated when creating the service credentials. Write access is required for creation and deletion tasks.
    - `<resource-instance-id>`: instance GUID of the {{site.data.keyword.iae_full_notm}} instance which you can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.

- Access the {{site.data.keyword.iae_full_notm}} service instance:
    ```javascript
    function getAnalyticsEngineById(instanceGuid) {
      IbmAnalyticsEngineServiceClient.getAnalyticsEngineById({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Get the state of the {{site.data.keyword.iae_full_notm}} cluster:
    ```javascript
    function getAnalyticsEngineStateById(instanceGuid) {
      IbmAnalyticsEngineServiceClient.getAnalyticsEngineStateById({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Create a customization request:
    ```javascript
    function createCustomizationRequest(instanceGuid) {
      // AnalyticsEngineCustomActionScript
      const analyticsEngineCustomActionScriptModel = {
        source_type: 'http',
        script_path: 'testString',
        source_props: { foo: 'bar' },
      };

      // AnalyticsEngineCustomAction
      const analyticsEngineCustomActionModel = {
        name: 'testString',
        type: 'bootstrap',
        script: analyticsEngineCustomActionScriptModel,
        script_params: ['testString'],
      };

      const target = 'all';
      const customActions = [analyticsEngineCustomActionModel];

      IbmAnalyticsEngineServiceClient.createCustomizationRequest({
        instanceGuid: instanceGuid,
        target: target,
        customActions: customActions,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Get all customization requests:
    ```javascript
    function getAllCustomizationRequests(instanceGuid) {
      IbmAnalyticsEngineServiceClient.getAllCustomizationRequests({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Get the customization requests by ID:
    ```javascript
    function getCustomizationRequestById(instanceGuid, requestId) {
      IbmAnalyticsEngineServiceClient.getCustomizationRequestById({
        instanceGuid: instanceGuid,
        requestId: requestId,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Resize the cluster:
    ```javascript
    function resizeCluster(instanceGuid, computeNodesCount) {
      IbmAnalyticsEngineServiceClient.resizeCluster({
        instanceGuid: instanceGuid,
        computeNodesCount: computeNodesCount,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Reset the cluster password:
    ```javascript
    function resetClusterPassword(instanceGuid) {
      IbmAnalyticsEngineServiceClient.resetClusterPassword({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Configure logging:
    ```javascript
    function configureLogging(instanceGuid) {
      // AnalyticsEngineLoggingNodeSpec
      const analyticsEngineLoggingNodeSpecModel = {
        node_type: 'management',
        components: ['ambari-server'],
      };

      // AnalyticsEngineLoggingServer
      const analyticsEngineLoggingServerModel = {
        type: 'logdna',
        credential: 'testString',
        api_host: 'testString',
        log_host: 'testString',
        owner: 'testString',
      };

      const logSpecs = [analyticsEngineLoggingNodeSpecModel];
      const logServer = analyticsEngineLoggingServerModel;

      IbmAnalyticsEngineServiceClient.configureLogging({
        instanceGuid: instanceGuid,
        logSpecs: logSpecs,
        logServer: logServer,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Get the log configuration:
    ```javascript
    function getLoggingConfig(instanceGuid) {
      IbmAnalyticsEngineServiceClient.getLoggingConfig({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(result)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

- Delete the log configuration:
    ```javascript
    function deleteLoggingConfig(instanceGuid) {
      IbmAnalyticsEngineServiceClient.deleteLoggingConfig({
        instanceGuid: instanceGuid,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(status)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock}

<!--
- Update private endpoint allowlist:
    ```javascript
    function updatePrivateEndpointWhitelist(instanceGuid) {
      IbmAnalyticsEngineServiceClient.updatePrivateEndpointWhitelist({
        instanceGuid: "{instanceGuid}",
        ipRanges: ipRanges,
        action: action,
      }).then((response) => {
        const { result, status, headers, statusText } = response;
        console.log(status)
      }).catch((err) => {
        console.log(JSON.stringify(err, null, 4));
      });
    }
    ```
    {: codeblock} -->
