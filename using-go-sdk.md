---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-22"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Using the Go SDK
{: #using-go}

The {{site.data.keyword.iae_full_notm}} Go SDK allows you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.

The source code for the SDK can be found at [GitHub](https://github.com/IBM/ibm-iae-go-sdk){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

## Getting the SDK
{: #get-go-sdk}

You need to download and install the SDK to use it in your Go applications. You can do this by entering the following command:
```
go get github.com/IBM/ibm-iae-go-sdk
```

If your application uses Go modules, you can add a suitable import to your Go application, and run:
```
go mod tidy
```

## Importing packages
{: #go-import-packages}

After you have installed the SDK, you need to import the SDK packages that you want to use in your Go applications.

For example:
```
import (
    "github.com/IBM/go-sdk-core/v3/core"
    "github.com/IBM/ibm-iae-go-sdk/ibmanalyticsengineapiv2"
)
```

## Creating a client and sourcing credentials
{: #go-client-credentials}

When you connect to {{site.data.keyword.iae_full_notm}}, a client is created and configured using the credential information (API key and service instance ID) that you provide. If you don't provide this information manually, these credentials can be sourced from a credentials file or from environment variables.

You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints){: external}.

To use the {{site.data.keyword.iae_full_notm}} Go SDK, you need the following values:

- `IAM_API_KEY`: The API key generated when creating the service credentials. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `instance_guid`: The value in `resource_instance_id` generated when the service credentials are created. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `IAE_ENDPOINT_URL` : The service endpoint URL including the `https://` protocol. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints){: external}.

## Initializing the configuration
{: #go-init-config}

The Go SDK allows you to construct the service client in one of two ways by:

- By setting client options programmatically

  You can construct an instance of the {{site.data.keyword.iae_full_notm}} service client by specifying various client options, like the authenticator and service endpoint URL, programmatically:

  ```
  import (
      "github.com/IBM/go-sdk-core/v3/core"
      "github.com/IBM/ibm-iae-go-sdk/ibmanalyticsengineapiv2"
  )

  // Create an IAM authenticator.
  authenticator := &core.IamAuthenticator{
      ApiKey: "<IAM_API_KEY>", // eg "0viPHOY7LbLNa9eLftrtHPpTjoGv6hbLD1QalRXikliJ"
  }

  // Construct an "options" struct for creating the service client.
  options := &ibmanalyticsengineapiv2.IbmAnalyticsEngineApiV2Options{
      Authenticator: authenticator,                               
      URL: "<IAE_ENDPOINT_URL>",  // eg "https://api.us-south.ae.cloud.ibm.com"
  }

  // Construct the service client.
  service, err := ibmanalyticsengineapiv2.NewIbmAnalyticsEngineApiV2(options)
  if err != nil {
      panic(err)
  }

  // Service operations can now be invoked using the "service" variable.
  ```
- By using external configuration properties

  To avoid hard-coding sourcing credentials, you can store these values in configuration properties outside of your application.

  To use configuration properties:

  1. Define the configuration properties to be used by your application. These properties can be implemented as:

    - Exported environment variables
    - Values stored in a credentials file

    The following example shows using environment variables. Each environment variable must be prefixed by `IBM_ANALYTICS_ENGINE_API`.

    ```
    export IBM_ANALYTICS_ENGINE_API_URL=<IAE_ENDPOINT_URL>
    export IBM_ANALYTICS_ENGINE_API_AUTH_TYPE=iam
    export IBM_ANALYTICS_ENGINE_API_APIKEY=<IAM_API_KEY>
    ```
    `IBM_ANALYTICS_ENGINE_API` is the default service name for the {{site.data.keyword.iae_full_notm}} API  client which means that the SDK will by default look for properties that start with this prefix folded to upper case.
  1. Build the service client:
    ```
    // Construct service client via config properties using default //service name ("ibm_analytics_engine_api")
    service, err := ibmanalyticsengineapiv2.NewIbmAnalyticsEngineApiV2UsingExternalConfig(
          &ibmanalyticsengineapiv2.IbmAnalyticsEngineApiV2Options{})
    if err != nil {
        panic(err)
    }
    ```

      The function `NewIbmAnalyticsEngineApiV2UsingExternalConfig`:

      - Constructs an IAM authenticator using the API key defined in the corresponding environment variable
      - Initializes the service client to use the service endpoint URL, also defined the corresponding environment variable

## Code samples using `iaesdk`
{: #code-samples-go}

The following code samples show how to:

- Access the {{site.data.keyword.iae_full_notm}} service instance:
  ```go
  func main() {

      // Construct GetAnalyticsEngineByIdOptions model
      getAnalyticsEngineByIdOptionsModel := new(ibmanalyticsengineapiv2.GetAnalyticsEngineByIdOptions)
      getAnalyticsEngineByIdOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      _, response, _ := service.GetAnalyticsEngineByID(getAnalyticsEngineByIdOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Get the state of the {{site.data.keyword.iae_full_notm}} cluster:
  ```go
  func main() {

      // Construct an instance of the GetAnalyticsEngineStateByIdOptions model
      getAnalyticsEngineStateByIdOptionsModel := new(ibmanalyticsengineapiv2.GetAnalyticsEngineStateByIdOptions)
      getAnalyticsEngineStateByIdOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      _, response, _ := service.GetAnalyticsEngineStateByID(getAnalyticsEngineStateByIdOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Create a customization request:
  ```go
  func main() {

      // Construct an instance of the AnalyticsEngineCustomActionScript model
      analyticsEngineCustomActionScriptModel := new(ibmanalyticsengineapiv2.AnalyticsEngineCustomActionScript)
      analyticsEngineCustomActionScriptModel.SourceType = core.StringPtr("http")
      analyticsEngineCustomActionScriptModel.ScriptPath = core.StringPtr("testString")
      analyticsEngineCustomActionScriptModel.SourceProps = core.StringPtr("testString")

      // Construct an instance of the AnalyticsEngineCustomAction model
      analyticsEngineCustomActionModel := new(ibmanalyticsengineapiv2.AnalyticsEngineCustomAction)
      analyticsEngineCustomActionModel.Name = core.StringPtr("testString")
      analyticsEngineCustomActionModel.Type = core.StringPtr("bootstrap")
      analyticsEngineCustomActionModel.Script = analyticsEngineCustomActionScriptModel
      analyticsEngineCustomActionModel.ScriptParams = []string{"testString"}

      // Construct an instance of the CreateCustomizationRequestOptions model
      createCustomizationRequestOptionsModel := new(ibmanalyticsengineapiv2.CreateCustomizationRequestOptions)
      createCustomizationRequestOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)
      createCustomizationRequestOptionsModel.Target = core.StringPtr("all")
      createCustomizationRequestOptionsModel.CustomActions = []ibmanalyticsengineapiv2.AnalyticsEngineCustomAction{*analyticsEngineCustomActionModel}

      // Invoke operation with valid options model (positive test)
      _, response, _ := service.CreateCustomizationRequest(createCustomizationRequestOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Get all customization requests:
  ```go
  func main() {

      // Construct an instance of the GetAllCustomizationRequestsOptions model
      getAllCustomizationRequestsOptionsModel := new(ibmanalyticsengineapiv2.GetAllCustomizationRequestsOptions)
      getAllCustomizationRequestsOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      // Invoke operation with valid options model
      _, response, _ := service.GetAllCustomizationRequests(getAllCustomizationRequestsOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Get the customization requests by ID:
  ```go
  func main() {

      // Construct an instance of the GetCustomizationRequestByIdOptions model
      getCustomizationRequestByIdOptionsModel := new(ibmanalyticsengineapiv2.GetCustomizationRequestByIdOptions)
      getCustomizationRequestByIdOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)
      getCustomizationRequestByIdOptionsModel.RequestID = core.StringPtr("RequestID")

      // Invoke operation with valid options model (positive test)
      _, response, _ := service.GetCustomizationRequestByID(getCustomizationRequestByIdOptionsModel)
      fmt.Println(response)
  }
  ```   
  {: codeblock}
- Resize the cluster:
  ```go
  func main() {

      // Construct an instance of the ResizeClusterOptions model
      resizeClusterOptionsModel := new(ibmanalyticsengineapiv2.ResizeClusterOptions)
      resizeClusterOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)
      resizeClusterOptionsModel.ComputeNodesCount = core.Int64Ptr(int64(computeNodesCount))

      // Invoke operation with valid options model (positive test)
      _, response, _ := service.ResizeCluster(resizeClusterOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Reset the cluster password:
  ```go
  func main() {

      // Construct an instance of the ResetClusterPasswordOptions model
      resetClusterPasswordOptionsModel := new(ibmanalyticsengineapiv2.ResetClusterPasswordOptions)
      resetClusterPasswordOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      // Invoke operation with valid options model (positive test)
      _, response, _ := service.ResetClusterPassword(resetClusterPasswordOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Configure logging:
  ```go
  func main() {

      // Construct an instance of the AnalyticsEngineLoggingNodeSpec model
      analyticsEngineLoggingNodeSpecModel := new(ibmanalyticsengineapiv2.AnalyticsEngineLoggingNodeSpec)
      analyticsEngineLoggingNodeSpecModel.NodeType = core.StringPtr("management")
      analyticsEngineLoggingNodeSpecModel.Components = []string{"ambari-server"}

      // Construct an instance of the AnalyticsEngineLoggingServer model
      analyticsEngineLoggingServerModel := new(ibmanalyticsengineapiv2.AnalyticsEngineLoggingServer)
      analyticsEngineLoggingServerModel.Type = core.StringPtr("logdna")
      analyticsEngineLoggingServerModel.Credential = core.StringPtr("testString")
      analyticsEngineLoggingServerModel.ApiHost = core.StringPtr("testString")
      analyticsEngineLoggingServerModel.LogHost = core.StringPtr("testString")
      analyticsEngineLoggingServerModel.Owner = core.StringPtr("testString")

      // Construct an instance of the ConfigureLoggingOptions model
      configureLoggingOptionsModel := new(ibmanalyticsengineapiv2.ConfigureLoggingOptions)
      configureLoggingOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)
      configureLoggingOptionsModel.LogSpecs = []ibmanalyticsengineapiv2.AnalyticsEngineLoggingNodeSpec{*analyticsEngineLoggingNodeSpecModel}
    configureLoggingOptionsModel.LogServer = analyticsEngineLoggingServerModel

      // Invoke operation with valid options model (positive test)
      response, _ := service.ConfigureLogging(configureLoggingOptionsModel)
      fmt.Println(response.StatusCode)
  }
  ```
  {: codeblock}
- Get the log configuration:
  ```go
  func main() {

      // Construct an instance of the GetLoggingConfigOptions model
      getLoggingConfigOptionsModel := new(ibmanalyticsengineapiv2.GetLoggingConfigOptions)
      getLoggingConfigOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      // Invoke operation with valid options model (positive test)
      _, response, _ := service.GetLoggingConfig(getLoggingConfigOptionsModel)
      fmt.Println(response)
  }
  ```
  {: codeblock}
- Delete the log configuration:
  ```go
  func main() {

      // Construct an instance of the DeleteLoggingConfigOptions model
      deleteLoggingConfigOptionsModel := new(ibmanalyticsengineapiv2.DeleteLoggingConfigOptions)
      deleteLoggingConfigOptionsModel.InstanceGuid = core.StringPtr(instanceGuid)

      // Invoke operation with valid options model (positive test)
      response, _ := service.DeleteLoggingConfig(deleteLoggingConfigOptionsModel)
      fmt.Println(response.StatusCode)
  }
  ```
  {: codeblock}
