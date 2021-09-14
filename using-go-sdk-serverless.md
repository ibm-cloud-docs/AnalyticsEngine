---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-13"

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
{: #using-go-serverless}

The {{site.data.keyword.iae_full_notm}} Go SDK allows you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API for serverless instances.

You can get the source code for the SDK from GitHub. See [
ibm-iae-go-sdk](https://github.com/IBM/ibm-iae-go-sdk){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

## Getting the SDK
{: #get-go-sdk}

You need to download and install the SDK to use it in your Go applications. You can do this by entering the following command:
```
go get -u github.com/IBM/ibm-iae-go-sdk
```
{:codeblock}

If your application uses Go modules, you can add a suitable import to your Go application, and run:
```
go mod tidy
```
{:codeblock}

## Importing packages
{: #go-import-packages}

After you have installed the SDK, you need to import the SDK packages that you want to use in your Go applications.

For example:
```
import (
  "github.com/IBM/go-sdk-core/v3/core"
  "github.com/IBM/ibm-iae-go-sdk/ibmanalyticsengineapiv3"
)
```
{:codeblock}

## Creating a client and sourcing credentials
{: #go-client-credentials}

When you connect to {{site.data.keyword.iae_full_notm}}, a client is created and configured using the credential information (API key and service instance ID) that you provide. If you don't provide this information manually, these credentials can be sourced from a credentials file or from environment variables.

You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless){: external}.

To use the {{site.data.keyword.iae_full_notm}} Go SDK, you need the following values:

- `IAM_API_KEY`: The API key generated when creating the service credentials. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `instance_guid`: The value in `resource_instance_id` generated when the service credentials are created. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `IAE_ENDPOINT_URL` : The service endpoint URL including the `https://` protocol. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3?code=java#service-endpoints){: external}.

## Initializing the configuration
{: #go-init-config}

The Go SDK allows you to construct the service client in one of two ways by:

- By setting client options programmatically

    You can construct an instance of the {{site.data.keyword.iae_full_notm}} service client by specifying various client options, like the authenticator and service endpoint URL, programmatically:

    ```go
    import (
    "github.com/IBM/go-sdk-core/v3/core"
    "github.com/IBM/ibm-iae-go-sdk/ibmanalyticsengineapiv3"
    )

    func main() {
        // Create an IAM authenticator.
        authenticator := &core.IamAuthenticator{
            ApiKey: "{apikey}", // eg "0viPHOY7LbLNa9eLftrtHPpTjoGv6hbLD1QalRXikliJ"
        }

        // Construct an "options" struct for creating the service client.
        options := &ibmanalyticsengineapiv3.IbmAnalyticsEngineApiV3Options{
            Authenticator: authenticator,
            URL: "{url}",  // eg "https://api.us-south.ae.cloud.ibm.com"
        }

        // Construct the service client.
        ibmAnalyticsEngineApiService, err := ibmanalyticsengineapiv3.NewIbmAnalyticsEngineApiV3(options)
        if err != nil {
            panic(err)
        }

        // Service operations can now be invoked using the "ibmAnalyticsEngineApiService" variable.
    }
    ```
    {: codeblock}

- By using external configuration properties

    To avoid hard-coding sourcing credentials, you can store these values in configuration properties outside of your application.

    To use configuration properties:

    1. Define the configuration properties to be used by your application. These properties can be implemented as:

        - Exported environment variables
        - Values stored in a credentials file

        The following example shows using environment variables. Each environment variable must be prefixed by `IBM_ANALYTICS_ENGINE_API`.

        ```sh
        export IBM_ANALYTICS_ENGINE_API_URL=<IAE_ENDPOINT_URL>
        export IBM_ANALYTICS_ENGINE_API_AUTH_TYPE=iam
        export IBM_ANALYTICS_ENGINE_API_APIKEY=<IAM_API_KEY>
        ```
        {: codeblock}

        `IBM_ANALYTICS_ENGINE_API` is the default service name for the {{site.data.keyword.iae_full_notm}} API  client which means that the SDK will by default look for properties that start with this prefix folded to uppercase.
    1. Build the service client:

        ```go
        // Create an IAM authenticator.
        authenticator := &core.IamAuthenticator{
           ApiKey: "{apikey}", // eg "0viPHOY7LbLNa9eLftrtHPpTjoGv6hbLD1QalRXikliJ"
        }

        // Construct an "options" struct for creating the service client.
        options := &ibmanalyticsengineapiv3.IbmAnalyticsEngineApiV3Options{
           Authenticator: authenticator,
           URL: "{url}",  // eg "https://api.us-south.ae.cloud.ibm.com"
        }

        // Construct the service client.
        service, err := ibmanalyticsengineapiv3.NewIbmAnalyticsEngineApiV3(options)
        if err != nil {
           panic(err)
        }
        ```
        {: codeblock}

## Code samples using `iaesdk`
{: #code-samples-go}

The following code samples show how to:

- Retrieve the details of a single instance:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetInstance(getInstanceOptions *GetInstanceOptions) (result *Instance, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetInstanceOptions model
        getInstanceOptionsModel := new(ibmanalyticsengineapiv3.GetInstanceOptions)
        getInstanceOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")

        _, response, _ := ibmAnalyticsEngineApiService.GetInstance(getInstanceOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}  

- Deploy a Spark application on a given serverless Spark instance:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) CreateApplication(createApplicationOptions *CreateApplicationOptions) (result *ApplicationResponse, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Sample request:
    ```
    func main() {
        // Construct an instance of the CreateApplicationOptions model
        createApplicationOptionsModel := new(ibmanalyticsengineapiv3.CreateApplicationOptions)
        createApplicationOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        createApplicationOptionsModel.Application = core.StringPtr("cos://bucket_name.my_cos/my_spark_app.py")
        createApplicationOptionsModel.Class = core.StringPtr("IbmAnalyticsEngineApi")
        createApplicationOptionsModel.Arguments = []string{"/opt/ibm/spark/examples/src/main/resources/people.txt"}
        createApplicationOptionsModel.Conf = make(map[string]interface{})
        createApplicationOptionsModel.Env = make(map[string]interface{})

        _, response, _ := ibmAnalyticsEngineApiService.CreateApplication(createApplicationOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Retrieve all Spark applications:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) ListApplications(listApplicationsOptions *ListApplicationsOptions) (result *ApplicationCollection, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the ListApplicationsOptions model
        listApplicationsOptionsModel := new(ibmanalyticsengineapiv3.ListApplicationsOptions)
        listApplicationsOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")

        _, response, _ := ibmAnalyticsEngineApiService.ListApplications(listApplicationsOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Retrieve the details of a given Spark application:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetApplication(getApplicationOptions *GetApplicationOptions) (result *ApplicationGetResponse, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetApplicationOptions model
        getApplicationOptionsModel := new(ibmanalyticsengineapiv3.GetApplicationOptions)
        getApplicationOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        getApplicationOptionsModel.ApplicationID = core.StringPtr("db933645-0b68-4dcb-80d8-7b71a6c8e542")

        _, response, _ := ibmAnalyticsEngineApiService.GetApplication(getApplicationOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Stop a running application identified by the `app_id` identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) DeleteApplication(deleteApplicationOptions *DeleteApplicationOptions) (response *core.DetailedResponse, err error)
    ```   
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the DeleteApplicationOptions model
        deleteApplicationOptionsModel := new(ibmanalyticsengineapiv3.DeleteApplicationOptions))
        deleteApplicationOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        deleteApplicationOptionsModel.ApplicationID = core.StringPtr("db933645-0b68-4dcb-80d8-7b71a6c8e542")

        _, response, _ := ibmAnalyticsEngineApiService.DeleteApplication(deleteApplicationOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Return the state of a given application:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetApplicationState(getApplicationStateOptions *GetApplicationStateOptions) (result *ApplicationGetStateResponse, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetApplicationStateOptions model
        getApplicationStateOptionsModel := new(ibmanalyticsengineapiv3.GetApplicationStateOptions)
        getApplicationStateOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        getApplicationStateOptionsModel.ApplicationID = core.StringPtr("db933645-0b68-4dcb-80d8-7b71a6c8e542")

        _, response, _ := ibmAnalyticsEngineApiService.GetApplicationState(getApplicationStateOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}    
