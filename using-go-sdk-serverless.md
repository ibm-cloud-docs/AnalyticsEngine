---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-27"

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
  "github.com/IBM/ibm-iae-go-sdk/ibmanalyticsengineapiv3"
)
```

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

    ```
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
        service, err := ibmanalyticsengineapiv3.NewIbmAnalyticsEngineApiV3(options)
        if err != nil {
            panic(err)
        }

        // Service operations can now be invoked using the "service" variable.
    }
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
        `IBM_ANALYTICS_ENGINE_API` is the default service name for the {{site.data.keyword.iae_full_notm}} API  client which means that the SDK will by default look for properties that start with this prefix folded to uppercase.
    1. Build the service client:
        ```
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

## Code samples using `iaesdk`
{: #code-samples-go}

The following code samples show how to:

- Retrieve the details of a single instance:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetInstanceByID(getInstanceByIdOptions *GetInstanceByIdOptions) (result *InstanceDetails, response *core.DetailedResponse, err error)
    }
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetInstanceByIdOptions model
        getInstanceByIdOptionsModel := new(ibmanalyticsengineapiv3.GetInstanceByIdOptions)
        getInstanceByIdOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")

        _, response, _ := service.GetInstanceByID(getInstanceByIdOptionsModel)
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
        createApplicationOptionsModel.Application = core.StringPtr("cos://ae-bucket-do-not-delete-dc0e9889-eab2-4t9e-9441-566209499546.s3.us-south.cloud-object-storage.appdomain.cloud/my_spark_application.py")
        createApplicationOptionsModel.Class = core.StringPtr("IbmAnalyticsEngineApi")
        createApplicationOptionsModel.ApplicationArguments = []string{"/opt/ibm/spark/examples/src/main/resources/people.txt"}
        createApplicationOptionsModel.Conf = make(map[string]interface{})
        createApplicationOptionsModel.Env = make(map[string]interface{})

        _, response, _ := service.CreateApplication(createApplicationOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Retrieve all Spark applications run on a given instance:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetApplications(getApplicationsOptions *GetApplicationsOptions) (result *ApplicationCollection, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetApplicationsOptions model
        getApplicationsOptionsModel := new(ibmanalyticsengineapiv3.GetApplicationsOptions)
        getApplicationsOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")

        _, response, _ := service.GetApplications(getApplicationsOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codepage}

- Retrieve the details of a given Spark application:
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) GetApplicationByID(getApplicationByIdOptions *GetApplicationByIdOptions) (result *ApplicationGetResponse, response *core.DetailedResponse, err error)
    ```
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the GetApplicationByIdOptions model
        getApplicationByIdOptionsModel := new(ibmanalyticsengineapiv3.GetApplicationByIdOptions)
        getApplicationByIdOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        getApplicationByIdOptionsModel.ApplicationID = core.StringPtr("db933645-0b68-4dcb-80d8-7b71a6c8e542")

        _, response, _ := service.GetApplicationByID(getApplicationByIdOptionsModel)
        fmt.Println(response)
    }
    ```

- Stop a running application identified by the `app_id` identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
    ```
    (ibmAnalyticsEngineApi *IbmAnalyticsEngineApiV3) DeleteApplicationByID(deleteApplicationByIdOptions *DeleteApplicationByIdOptions) (response *core.DetailedResponse, err error)
    ```   
    {: codeblock}

    Example request:
    ```
    func main() {
        // Construct an instance of the DeleteApplicationByIdOptions model
        deleteApplicationByIdOptionsModel := new(ibmanalyticsengineapiv3.DeleteApplicationByIdOptions)
        deleteApplicationByIdOptionsModel.InstanceID = core.StringPtr("dc0e9889-eab2-4t9e-9441-566209499546")
        deleteApplicationByIdOptionsModel.ApplicationID = core.StringPtr("db933645-0b68-4dcb-80d8-7b71a6c8e542")

        _, response, _ := service.DeleteApplicationByID(deleteApplicationByIdOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}

- Return the status of the application identified by the `app_id` identifier:
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

        _, response, _ := service.GetApplicationState(getApplicationStateOptionsModel)
        fmt.Println(response)
    }
    ```
    {: codeblock}    
