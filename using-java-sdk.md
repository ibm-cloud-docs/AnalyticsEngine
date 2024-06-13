---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-13"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Using Java
{: #java}

The {{site.data.keyword.iae_full_notm}} SDK for Java provides features that allow you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.

The source code can be found in this [GitHub repository](https://github.com/IBM/ibm-iae-java-sdk){: external}.

## Getting the SDK
{: #java-install}

The easiest way to use the {{site.data.keyword.iae_full_notm}} Java SDK is to use Maven to manage library dependencies. If you aren't familiar with Maven, see [Maven in 5-Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html){: external}.

Maven uses a file that is called `pom.xml` to specify the libraries and their versions needed for a Java project. Here is an example of the `pom.xml` file for using the {{site.data.keyword.iae_full_notm}} Java SDK to connect to {{site.data.keyword.iae_full_notm}}.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.iae</groupId>
    <artifactId>my-iae</artifactId>
    <version>2.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>com.ibm.cloud</groupId>
            <artifactId>ibm-analytics-engine-api</artifactId>
            <version>0.0.5</version>
        </dependency>
    </dependencies>
</project>
```
{: codeblock}

## Creating a client and sourcing credentials
{: #java-client-credentials}

When you connect to {{site.data.keyword.iae_full_notm}}, a client is created and configured using the credential information (API key and service instance ID) that you provide. If you don't provide this information manually, these credentials can be sourced from a credentials file or from environment variables.

You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints){: external}.

To use the {{site.data.keyword.iae_full_notm}} Java SDK, you need the following values:

- `IAM_API_KEY`: The API key generated when creating the service credentials. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `instance_guid`: The value in `resource_instance_id` generated when the service credentials are created. You can get the value  by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `IAE_ENDPOINT_URL` : The service endpoint URL including the `https://` protocol. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints){: external}.

## Initializing the configuration
{: #java-init-config}

The Java SDK allows you to construct the service client in one of two ways by:

- By setting the client options programmatically

    You can construct an instance of the {{site.data.keyword.iae_full_notm}} service client by specifying various client options, like the authenticator and service endpoint URL, programmatically:
    ```java
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v2.IbmAnalyticsEngineApi;
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v2.model.*;
    import com.ibm.cloud.sdk.core.http.Response;
    import com.ibm.cloud.sdk.core.security.*;

    private static String IAM_API_KEY = "<api-key>"; // eg "0viPHOY7LbLNa9eLftrtHPpTjoGv6hbLD1QalRXikliJ"
    private static String IAE_ENDPOINT_URL = "<endpoint>"; // Current list avaiable at https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints
    private static String instance_guid = "<resource-instance-id>";

    public static void main(String[] args)
    {
        try {
            // Create an IAM authenticator.
            Authenticator authenticator = new IamAuthenticator(IAM_API_KEY);

            // Construct the service client.
            service = new IbmAnalyticsEngineApi(IbmAnalyticsEngineApi.DEFAULT_SERVICE_NAME, authenticator);
            // Set our service URL.
            service.setServiceUrl(IAE_ENDPOINT_URL);

        } catch (Exception e) {
            System.out.println("Exception");
        }
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

        ```java
        export IBM_ANALYTICS_ENGINE_API_URL=<IAE_ENDPOINT_URL>
        export IBM_ANALYTICS_ENGINE_API_AUTH_TYPE=iam
        export IBM_ANALYTICS_ENGINE_API_APIKEY=<IAM_API_KEY>
        ```
        `IBM_ANALYTICS_ENGINE_API` is the default service name for the {{site.data.keyword.iae_full_notm}} API client which means that the SDK will by default look for properties that start with this prefix.
    1. Build the service client:
        ```java
        import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v2.IbmAnalyticsEngineApi;
        import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v2.model.*;
        import com.ibm.cloud.sdk.core.http.Response;
        import com.ibm.cloud.sdk.core.security.*;

        IbmAnalyticsEngineApi service = IbmAnalyticsEngineApi.newInstance();
        ```
        {: codeblock}

        The function `IbmAnalyticsEngineApi.newInstance()`:

        - Constructs an IAM authenticator using the API key defined in the corresponding environment variable
        - Initializes the service client to use the service endpoint URL, also defined the corresponding environment variable

## Code Samples
{: #code-samples-java}

The following code samples show how to:

- Access the {{site.data.keyword.iae_full_notm}} service instance:
    ```java
    public static void getAnalyticsEngineByIdWOptions() {
        try {
            // Construct an instance of the GetAnalyticsEngineByIdOptions model
            GetAnalyticsEngineByIdOptions getAnalyticsEngineByIdOptionsModel = new GetAnalyticsEngineByIdOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngine> response = service.getAnalyticsEngineById(getAnalyticsEngineByIdOptionsModel).execute();
            AnalyticsEngine responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));                

        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Get the state of the {{site.data.keyword.iae_full_notm}} cluster:
    ```java
    public static void getAnalyticsEngineStateByIdWOptions() {

        try {
            // Construct an instance of the GetAnalyticsEngineStateByIdOptions model
            GetAnalyticsEngineStateByIdOptions getAnalyticsEngineStateByIdOptionsModel = new GetAnalyticsEngineStateByIdOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            Response<AnalyticsEngineState> response = service.getAnalyticsEngineStateById(getAnalyticsEngineStateByIdOptionsModel).execute();
            AnalyticsEngineState responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Create a customization request:
    ```java
    public static void createCustomizationRequestWOptions() {

        try {
            // Construct an instance of the AnalyticsEngineCustomActionScript model
            AnalyticsEngineCustomActionScript analyticsEngineCustomActionScriptModel = new AnalyticsEngineCustomActionScript.Builder()
            .sourceType("http")
            .scriptPath("testString")
            .sourceProps(new java.util.HashMap<String,Object>(){{put("foo", "testString"); }})
            .build();

            // Construct an instance of the AnalyticsEngineCustomAction model
            AnalyticsEngineCustomAction analyticsEngineCustomActionModel = new AnalyticsEngineCustomAction.Builder()
            .name("testString")
            .type("bootstrap")
            .script(analyticsEngineCustomActionScriptModel)
            .scriptParams(new ArrayList<String>(Arrays.asList("testString")))
            .build();

            // Construct an instance of the CreateCustomizationRequestOptions model
            CreateCustomizationRequestOptions createCustomizationRequestOptionsModel = new CreateCustomizationRequestOptions.Builder()
            .instanceGuid(instanceGuid)
            .target("all")
            .customActions(new ArrayList<AnalyticsEngineCustomAction>(Arrays.asList(analyticsEngineCustomActionModel)))
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngineCreateCustomizationResponse> response = service.createCustomizationRequest(createCustomizationRequestOptionsModel).execute();
            System.out.println(Integer.toString(response.getStatusCode()));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Get all customization requests:
    ```java
    public static void getAllCustomizationRequestsWOptions() {

        try {
            // Construct an instance of the GetAllCustomizationRequestsOptions model
            GetAllCustomizationRequestsOptions getAllCustomizationRequestsOptionsModel = new GetAllCustomizationRequestsOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<List<AnalyticsEngineCustomizationRequestCollectionItem>> response = service.getAllCustomizationRequests(getAllCustomizationRequestsOptionsModel).execute();
            List<AnalyticsEngineCustomizationRequestCollectionItem> responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Get the customization requests by ID:
    ```java
    public static void getCustomizationRequestByIdWOptions(String requestId) {

        try {
            // Construct an instance of the GetCustomizationRequestByIdOptions model
            GetCustomizationRequestByIdOptions getCustomizationRequestByIdOptionsModel = new GetCustomizationRequestByIdOptions.Builder()
            .instanceGuid(instanceGuid)
            .requestId(requestId)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngineCustomizationRunDetails> response = service.getCustomizationRequestById(getCustomizationRequestByIdOptionsModel).execute();
            AnalyticsEngineCustomizationRunDetails responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Resize the cluster:
    ```java
    public static void resizeClusterWOptions(String computeNodesCount) {

        try {
            // Construct an instance of the ResizeClusterOptions model
            ResizeClusterOptions resizeClusterOptionsModel = new ResizeClusterOptions.Builder()
            .instanceGuid(instanceGuid)
            .computeNodesCount(Long.valueOf(computeNodesCount))
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngineResizeClusterResponse> response = service.resizeCluster(resizeClusterOptionsModel).execute();
            System.out.println(Integer.toString(response.getStatusCode()));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Reset the cluster password:
    ```java
    public static void resetClusterPasswordWOptions() {

        try {
            // Construct an instance of the ResetClusterPasswordOptions model
            ResetClusterPasswordOptions resetClusterPasswordOptionsModel = new ResetClusterPasswordOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngineResetClusterPasswordResponse> response = service.resetClusterPassword(resetClusterPasswordOptionsModel).execute();
            AnalyticsEngineResetClusterPasswordResponse responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Configure logging:
    ```java
    public static void configureLoggingWOptions() {

        try {
            // Construct an instance of the AnalyticsEngineLoggingServer model
            AnalyticsEngineLoggingServer analyticsEngineLoggingServerModel = new AnalyticsEngineLoggingServer.Builder()
            .type("logdna")
            .credential("testString")
            .apiHost("testString")
            .logHost("testString")
            .owner("testString")
            .build();

            // Construct an instance of the AnalyticsEngineLoggingNodeSpec model
            AnalyticsEngineLoggingNodeSpec analyticsEngineLoggingNodeSpecModel = new AnalyticsEngineLoggingNodeSpec.Builder()
            .nodeType("management")
            .components(new ArrayList<String>(Arrays.asList("ambari-server")))
            .build();

            // Construct an instance of the ConfigureLoggingOptions model
            ConfigureLoggingOptions configureLoggingOptionsModel = new ConfigureLoggingOptions.Builder()
            .instanceGuid(instanceGuid)
            .logSpecs(new ArrayList<AnalyticsEngineLoggingNodeSpec>(Arrays.asList(analyticsEngineLoggingNodeSpecModel)))
            .logServer(analyticsEngineLoggingServerModel)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<Void> response = service.configureLogging(configureLoggingOptionsModel).execute();
            System.out.println(Integer.toString(response.getStatusCode()));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Get the log configuration:
    ```java
    public static void getLoggingConfigWOptions() {

        try {
            // Construct an instance of the GetLoggingConfigOptions model
            GetLoggingConfigOptions getLoggingConfigOptionsModel = new GetLoggingConfigOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<AnalyticsEngineLoggingConfigDetails> response = service.getLoggingConfig(getLoggingConfigOptionsModel).execute();
            AnalyticsEngineLoggingConfigDetails responseObj = response.getResult();
            System.out.println(String.valueOf(responseObj));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}

- Delete the log configuration:
    ```java
    public static void deleteLoggingConfigWOptions() {

        try {
            // Construct an instance of the DeleteLoggingConfigOptions model
            DeleteLoggingConfigOptions deleteLoggingConfigOptionsModel = new DeleteLoggingConfigOptions.Builder()
            .instanceGuid(instanceGuid)
            .build();

            // Invoke operation with valid options model (positive test)
            Response<Void> response = service.deleteLoggingConfig(deleteLoggingConfigOptionsModel).execute();
            System.out.println(Integer.toString(response.getStatusCode()));
        } catch (Exception e) {
            System.out.printf("Error: %s\n", e.getMessage());
        }
    }
    ```
    {: codeblock}


