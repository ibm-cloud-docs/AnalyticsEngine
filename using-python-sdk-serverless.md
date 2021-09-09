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

# Using the Python SDK
{: #using-python-sdk-serverless}

The {{site.data.keyword.iae_full_notm}} SDK can be installed by installing the library `iaesdk` from the Python Package Index.

Type the following command into a command line:
```
pip install --upgrade "iaesdk>=1.1.0"
```
{: codeblock}

Source code can be found at [GitHub](https://github.com/IBM/ibm-iae-python-sdk){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

You need to provide the service endpoints and the API key when you create a {{site.data.keyword.iae_full_notm}} service resource or a low-level client.

The service instance ID is also referred to as a instance GUID. You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints-serverless){: external}.

To use the `iaesdk` library, you need the following values:

- `IAM_API_KEY`: The API key generated when creating the service credentials. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `instance_guid`: The value in `resource_instance_id` generated when the service credentials are created. You can retrieve by viewing the service credentials on the [IBM Cloud dashboard](https://cloud.ibm.com/resources){: external}.
- `IAE_ENDPOINT_URL`: The service endpoint URL including the `https://` protocol. See [Service endpoints](https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints){: external}.

## Code samples using `iaesdk`
{: #code-samples-python-sdk}

Getting started with the Python SDK after you have installed it, involves sourcing credentials to the {{site.data.keyword.iae_full_notm}} service, invoking the service and then issuing different cluster commands as shown in the following sample code snippets. The code examples are written for Python 3.7.

The code samples show how to:

- Authenticate to the {{site.data.keyword.iae_full_notm}} service and build a service client:

    ```python
    from iaesdk import IbmAnalyticsEngineApiV3
    from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

    # Constants for IBM Analytics Engine values
    IAM_API_KEY = "{apikey}" # eg "W00YiRnLW4a3fTjMB-odB-2ySfTrFBIQQWanc--P3byk"
    IAE_ENDPOINT_URL = "{url}" # Current list available at https://cloud.ibm.com/apidocs/ibm-analytics-engine-v3?code=python#authentication

    # Create an IAM authenticator.
    authenticator = IAMAuthenticator(IAM_API_KEY)

    # Construct the service client.
    iaesdk_service = IbmAnalyticsEngineApiV3(authenticator=authenticator)

    # Set our custom service URL
    iaesdk_service.set_service_url(IAE_ENDPOINT_URL)

    # Service operations can now be invoked using the "iaesdk_service" variable.
    ```
    {: codeblock}

- Retrieve the details of a single instance:
    ```python
    get_instance_by_id(self,
        instance_id: str,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.get_instance_by_id(instance_id)
    print(response.result)
    ```
    {: codeblock}    

- Deploy a Spark application on a given serverless Spark instance:
    ```python
    create_application(self,
        instance_id: str,
        *,
        application_details: 'ApplicationRequestApplicationDetails' = None,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.create_application(instance_id)
    print(response.result)
    ```

- Retrieve all Spark applications run on a given instance:
    ```python
    get_applications(self,
        instance_id: str,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.get_applications(instance_id)
    print(response.result)
    ```
    {: codeblock}

- Retrieve the details of a given Spark application:
    ```python
    get_application_by_id(self,
        instance_id: str,
        application_id: str,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.get_application_by_id(instance_id, application_id)
    print(response.result)
    ```
    {: codeblock}

- Stop a running application identified by the `app_id` identifier. This is an idempotent operation. Performs no action if the requested application is already stopped or completed.
    ```python
    delete_application_by_id(self,
        instance_id: str,
        application_id: str,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.delete_application_by_id(instance_id, application_id)
    print(response.result)
    ```
    {: codeblock}

- Return the status of the application identified by the `app_id` identifier:

    ```python
    get_application_state(self,
        instance_id: str,
        application_id: str,
        **kwargs
    ) -> DetailedResponse
    ```
    {: codeblock}

    Example request:
    ```
    response = iaesdk_service.get_application_state(instance_id, application_id)
    print(response.result)
    ```
    {: codeblock}
