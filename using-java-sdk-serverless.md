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

# Using Java
{: #java-serverless}

The {{site.data.keyword.iae_full_notm}} SDK for Java provides features that allow you to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API for serverless instances.

The source code can be found in this [GitHub repository](https://github.com/IBM/ibm-iae-java-sdk){: external}.

## Getting the SDK
{: #java-install}

The easiest way to use the {{site.data.keyword.iae_full_notm}} Java SDK is to use Maven to manage library dependencies. If you aren't familiar with Maven, see [Maven in 5-Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html){: external}.

Maven uses a file that is called `pom.xml` to specify the libraries and their versions needed for a Java project. Here is an example of the `pom.xml` file for using the {{site.data.keyword.iae_full_notm}} Java SDK to connect to {{site.data.keyword.iae_full_notm}}.

```xml
<dependency>
  <groupId>com.ibm.cloud</groupId>
  <artifactId>ibm-analytics-engine-api</artifactId>
  <version>0.4.0</version>
</dependency>
```
{:codeblock}

## Creating a client and sourcing credentials
{: #java-client-credentials}

When you connect to {{site.data.keyword.iae_full_notm}}, a client is created and configured using the credential information (API key and service instance ID) that you provide. If you don't provide this information manually, these credentials can be sourced from a credentials file or from environment variables.

You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless){: external}.

To use the {{site.data.keyword.iae_full_notm}} Java SDK, you need the following values:

- `IAM_API_KEY`: The API key generated when creating the service credentials. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `instance_guid`: The value in `resource_instance_id` generated when the service credentials are created. You can get the value  by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `IAE_ENDPOINT_URL` : The service endpoint URL including the `https://` protocol. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3?code=java#service-endpoints){: external}.

## Initializing the configuration
{: #java-init-config}

The Java SDK allows you to construct the service client in one of two ways by:

- By setting the client options programmatically

    You can construct an instance of the {{site.data.keyword.iae_full_notm}} service client by specifying various client options, like the authenticator and service endpoint URL, programmatically:
    ```java
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.IbmAnalyticsEngineApi;
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.model.*;
    import com.ibm.cloud.sdk.core.http.Response;
    import com.ibm.cloud.sdk.core.security.*;

    private static IbmAnalyticsEngineApi ibmAnalyticsEngineApiService;

    private static String IAM_API_KEY = "{apikey}";
    private static String IAE_ENDPOINT_URL = "{url}";

    public static void main(String[] args)
    {
        try {
            // Create an IAM authenticator.
            Authenticator authenticator = new IamAuthenticator(IAM_API_KEY);
            // Construct the service client.
            ibmAnalyticsEngineApiService = new IbmAnalyticsEngineApi(IbmAnalyticsEngineApi.DEFAULT_SERVICE_NAME, authenticator);
            // Set our service URL.
            ibmAnalyticsEngineApiService.setServiceUrl(IAE_ENDPOINT_URL);

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

        ```
        export IBM_ANALYTICS_ENGINE_API_URL=<IAE_ENDPOINT_URL>
        export IBM_ANALYTICS_ENGINE_API_AUTH_TYPE=iam
        export IBM_ANALYTICS_ENGINE_API_APIKEY=<IAM_API_KEY>
        ```
        `IBM_ANALYTICS_ENGINE_API` is the default service name for the {{site.data.keyword.iae_full_notm}} API client which means that the SDK will by default look for properties that start with this prefix.
    1. Build the service client:
        ```java
        import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.IbmAnalyticsEngineApi;
        import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.model.*;
        import com.ibm.cloud.sdk.core.http.Response;
        import com.ibm.cloud.sdk.core.security.*;

        // Create an IAM authenticator.
        Authenticator authenticator = new IamAuthenticator(IAM_API_KEY);
        // Construct the service client.
        ibmAnalyticsEngineApiService = new IbmAnalyticsEngineApi(IbmAnalyticsEngineApi.DEFAULT_SERVICE_NAME, authenticator);
        // Set our service URL.
        ibmAnalyticsEngineApiService.setServiceUrl(IAE_ENDPOINT_URL);
        ```
        {: codeblock}

## Code Samples
{: #code-samples-java}

The following code samples show how to:

- Access the {{site.data.keyword.iae_full_notm}} service instance:
    ```java
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.IbmAnalyticsEngineApi;
    import com.ibm.cloud.iaesdk.ibm_analytics_engine_api.v3.model.*;
    import com.ibm.cloud.sdk.core.http.Response;
    import com.ibm.cloud.sdk.core.security.*;

    private static IbmAnalyticsEngineApi ibmAnalyticsEngineApiService;

    private static String IAM_API_KEY = "{apikey}";
    private static String IAE_ENDPOINT_URL = "{url}";

    public static void main(String[] args)
    {
        try {
            // Create an IAM authenticator.
            Authenticator authenticator = new IamAuthenticator(IAM_API_KEY);
            // Construct the service client.
            ibmAnalyticsEngineApiService = new IbmAnalyticsEngineApi(IbmAnalyticsEngineApi.DEFAULT_SERVICE_NAME, authenticator);
            // Set our service URL.
            ibmAnalyticsEngineApiService.setServiceUrl(IAE_ENDPOINT_URL);

        } catch (Exception e) {
            System.out.println("Exception");
        }
    }
    ```
    {: codeblock}

- Retrieve the details of a single instance.:
    ```java
    ServiceCall<InstanceDetails> getInstanceById(GetInstanceByIdOptions getInstanceByIdOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the GetInstanceByIdOptions model
    GetInstanceByIdOptions getInstanceByIdOptionsModel = new GetInstanceByIdOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .build();

    // Invoke operation with valid options model (positive test)
    Response<InstanceDetails> response = ibmAnalyticsEngineApiService.getInstanceById(getInstanceByIdOptionsModel).execute();
    InstanceDetails responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```

- Deploy a Spark application on a given serverless Spark instancet:
    ```java
    ServiceCall<ApplicationResponse> createApplication(CreateApplicationOptions createApplicationOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the ApplicationRequestApplicationDetails model
    ApplicationRequestApplicationDetails applicationRequestApplicationDetailsModel = new ApplicationRequestApplicationDetails.Builder()
    .application("cos://ae-bucket-do-not-delete-dc0e9889-eab2-4t9e-9441-566209499546.s3.us-south.cloud-object-storage.appdomain.cloud/my_spark_application.py")
    .xClass("IbmAnalyticsEngineApi")
    .applicationArguments(new java.util.ArrayList<String>(java.util.Arrays.asList("/opt/ibm/spark/examples/src/main/resources/people.txt")))
    .conf(new java.util.HashMap<String, Object>() { { put("spark.app.name", "MySparkApp"); } })
    .env(new java.util.HashMap<String, Object>() { { put("SPARK_ENV_LOADED", "2"); } })
    .build();

    // Construct an instance of the CreateApplicationOptions model
    CreateApplicationOptions createApplicationOptionsModel = new CreateApplicationOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .applicationDetails(applicationRequestApplicationDetailsModel)
    .build();

    // Invoke operation with valid options model (positive test)
    Response<ApplicationResponse> response = ibmAnalyticsEngineApiService.createApplication(createApplicationOptionsModel).execute();
    ApplicationResponse responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```

- Retrieve all Spark applications run on a given instance:
    ```java
    ServiceCall<ApplicationCollection> getApplications(GetApplicationsOptions getApplicationsOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the GetApplicationsOptions model
    GetApplicationsOptions getApplicationsOptionsModel = new GetApplicationsOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .build();

    // Invoke operation with valid options model (positive test)
    Response<ApplicationCollection> response = ibmAnalyticsEngineApiService.getApplications(getApplicationsOptionsModel).execute();
    ApplicationCollection responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```

- Retrieve the details of a given Spark application:
    ```java
    ServiceCall<ApplicationGetResponse> getApplicationById(GetApplicationByIdOptions getApplicationByIdOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the GetApplicationByIdOptions model
    GetApplicationByIdOptions getApplicationByIdOptionsModel = new GetApplicationByIdOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .applicationId("db933645-0b68-4dcb-80d8-7b71a6c8e542")
    .build();

    // Invoke operation with valid options model (positive test)
    Response<ApplicationGetResponse> response = ibmAnalyticsEngineApiService.getApplicationById(getApplicationByIdOptionsModel).execute();
    ApplicationGetResponse responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```

- Stop a running application identified by the `app_id` identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
    ```java
    ServiceCall<Void> deleteApplicationById(DeleteApplicationByIdOptions deleteApplicationByIdOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the DeleteApplicationByIdOptions model
    DeleteApplicationByIdOptions deleteApplicationByIdOptionsModel = new DeleteApplicationByIdOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .applicationId("db933645-0b68-4dcb-80d8-7b71a6c8e542")
    .build();

    // Invoke operation with valid options model (positive test)
    Response<Void> response = ibmAnalyticsEngineApiService.deleteApplicationById(deleteApplicationByIdOptionsModel).execute();
    Void responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```

- Return the status of the application identified by the `app_id` identifier:
    ```java
    ServiceCall<ApplicationGetStateResponse> getApplicationState(GetApplicationStateOptions getApplicationStateOptions)
    ```
    {: codeblock}

    Example request:
    ```
    // Construct an instance of the GetApplicationStateOptions model
    GetApplicationStateOptions getApplicationStateOptionsModel = new GetApplicationStateOptions.Builder()
    .instanceId("dc0e9889-eab2-4t9e-9441-566209499546")
    .applicationId("db933645-0b68-4dcb-80d8-7b71a6c8e542")
    .build();

    // Invoke operation with valid options model (positive test)
    Response<ApplicationGetStateResponse> response = ibmAnalyticsEngineApiService.getApplicationState(getApplicationStateOptionsModel).execute();
    ApplicationGetStateResponse responseObj = response.getResult();
    System.out.println(String.valueOf(responseObj));
    ```
