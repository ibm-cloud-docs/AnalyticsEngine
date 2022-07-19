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

# Using the Python SDK
{: #using-python-sdk}

The {{site.data.keyword.iae_full_notm}} SDK can be installed by installing the library `iaesdk` from the Python Package Index.

Type the following command into a command line:
```python
pip install iaesdk
```
{: codeblock}

Source code can be found at [GitHub](https://github.com/ibm/ibm-iae-python-sdk/){: external}. The `iaesdk` library provides complete access to the {{site.data.keyword.iae_full_notm}} API.

You need to provide the service endpoints and the API key when you create a {{site.data.keyword.iae_full_notm}} service resource or a low-level client.

The service instance ID is also referred to as a instance GUID. You can retrieve the service instance ID when you create service credentials or through the CLI. See [Retrieving service endpoints](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-endpoints){: external}.

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
    from iaesdk import IbmAnalyticsEngineApiV2
    from ibm_cloud_sdk_core.authenticators import IAMAuthenticator

    # Constants for IBM Analytics Engine values
    IAM_API_KEY = "<api-key>" # eg "W00YiRnLW4a3fTjMB-odB-2ySfTrFBIQQWanc--P3byk"
    IAE_ENDPOINT_URL = "<endpoint>" # Current list avaiable at https://cloud.ibm.com/apidocs/ibm-analytics-engine#service-endpoints

    # Create an IAM authenticator.
    authenticator = IAMAuthenticator(IAM_API_KEY)

    # Construct the service client.
    iaesdk_service = IbmAnalyticsEngineApiV2(authenticator=authenticator)

    # Set our custom service URL (optional)
    iaesdk_service.set_service_url(IAE_ENDPOINT_URL)

    # Service operations can now be invoked using the "iaesdk_service" variable.
    ```
    {: codeblock}

- Access the {{site.data.keyword.iae_full_notm}} service instance:
    ```python
    def get_analytics_engine_by_id(instance_guid):
        try:
            response = iaesdk_service.get_analytics_engine_by_id(instance_guid)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Get the state of the {{site.data.keyword.iae_full_notm}} cluster:
    ```python
    def get_analytics_engine_state_by_id(instance_guid):
        try:
            response = iaesdk_service.get_analytics_engine_state_by_id(instance_guid)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Create a customization request:
    ```python
    def create_customization_request(instance_guid):
        try:
            # Construct a dict representation of a AnalyticsEngineCustomActionScript model
            analytics_engine_custom_action_script_model =  {
                'source_type': 'http',
                'script_path': 'testString',
                'source_props': 'unknown type: object'
            }
            # Construct a dict representation of a AnalyticsEngineCustomAction model
            analytics_engine_custom_action_model =  {
                'name': 'testString',
                'type': 'bootstrap',
                'script': analytics_engine_custom_action_script_model,
                'script_params': ['testString']
            }

            # Set up parameter values
            target = 'all'
            custom_actions = [analytics_engine_custom_action_model]

            # Invoke method
            response = iaesdk_service.create_customization_request(
                instance_guid,
                target,
                custom_actions,
            )
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Get all customization requests:
    ```python
    def get_all_customization_requests(instance_guid):
        try:
            response = iaesdk_service.get_all_customization_requests(instance_guid)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Get the customization requests by ID:
    ```python
    def get_customization_request_by_id(instance_guid, request_id):
        try:
            response = iaesdk_service.get_customization_request_by_id(instance_guid, request_id)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}
    
- Resize the cluster:
    ```python
    def resize_cluster(instance_guid, compute_nodes_count):
        try:
            response = iaesdk_service.resize_cluster(instance_guid, compute_nodes_count)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Reset the cluster password:
    ```python
    def reset_cluster_password(instance_guid):
        try:
            response = iaesdk_service.reset_cluster_password(instance_guid)
            print(response.result)
        except Exception as e:
            print("Unable to reset: {0}".format(e))
    ```
    {: codeblock}

- Configure logging:
    ```python
    def configure_logging(instance_guid):
        # Construct a dict representation of a AnalyticsEngineLoggingNodeSpec model
        analytics_engine_logging_node_spec_model =  {
            'node_type': 'management',
            'components': ['ambari-server']
        }
        # Construct a dict representation of a AnalyticsEngineLoggingServer model
        analytics_engine_logging_server_model =  {
            'type': 'logdna',
            'credential': 'testString',
            'api_host': 'testString',
            'log_host': 'testString',
            'owner': 'testString'
        }

        # Set up parameter values
        log_specs = [analytics_engine_logging_node_spec_model]
        log_server = analytics_engine_logging_server_model
        try:
            response = iaesdk_service.configure_logging(instance_guid, log_specs, log_server)
            print(response.status_code)
        except Exception as e:
            print("Unable to configure: {0}".format(e))
    ```
    {: codeblock}

- Get the log configuration:
    ```python
    def get_logging_config(instance_guid):
        try:
            response = iaesdk_service.get_logging_config(instance_guid)
            print(response.result)
        except Exception as e:
            print("Unable to retrieve: {0}".format(e))
    ```
    {: codeblock}

- Delete the log configuration:
    ```python
    def delete_logging_config(instance_guid):
        try:
            response = iaesdk_service.delete_logging_config(instance_guid)
            print(response.status_code)
        except Exception as e:
            print("Unable to delete: {0}".format(e))
    ```
    {: codeblock} 
    
<!--
- Update private endpoint allowlist:
    ```python
    def update_private_endpoint_whitelist(instance_guid):
        try:
            response = iaesdk_service.update_private_endpoint_whitelist(instance_guid)
                 instance_guid,
                 ip_ranges,
                 action,
                )print(response.status_code)
        except Exception as e:
            print("Unable to update: {0}".format(e))
    ```
    {: codeblock} -->
