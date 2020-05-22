---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-19"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Whitelisting IP addresses to control network traffic
{: #whitelist-cluster-access}

You can control network traffic into your {{site.data.keyword.iae_full_notm}} instance based on the network (IP Address) from where the request is originated. Requests not originating from the whitelisted IP addresses are denied access.

This whitelisting feature is only applicable for the private endpoint-enabled instances. 

## Whitelisting IP addresses to control network traffic into the instance

When you provision an {{site.data.keyword.iae_full_notm}} instance, you can choose to create it with either public or private endpoints. If you use [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration), you can optionally whitelist IP address ranges to control incoming network traffic to your instance.

### Updating IP ranges in the whitelist

You can add or delete IP ranges to and from the whitelist of an {{site.data.keyword.iae_full_notm}} instance by invoking the `PATCH` operation on the `private_endpoint_whitelist` endpoint of the {{site.data.keyword.iae_full_notm}} cluster management API.

For example, if you created your service instance in the `US-South` {{site.data.keyword.Bluemix_notm}} region, enter:
```
curl --request PATCH  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/ <service_instance_guid>/private_endpoint_whitelist
\ -H "Authorization: Bearer <user's IAM token>"
\ -H "accept: application/json"
\ -H "Content-type: application/json"
\ -d <whitelist-payload.json>
```
{: codeblock}


### Configuring IP ranges to be whitelisted

You configure whitelisting on an  {{site.data.keyword.iae_full_notm}} cluster by invoking the `POST` operation on the `update_private_endpoint_whitelist` endpoint of the {{site.data.keyword.iae_full_notm}} cluster management API.

For example, if you created your service instance cluster in the `US-South` {{site.data.keyword.Bluemix_notm}} region, enter:
```
curl --request POST  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/ <service_instance_guid>/update_private_endpoint_whitelist
\ -H "Authorization: Bearer $TOKEN"
\ -H "application/json"
\ -H "Content-type:	 application/json"
\ -d <whitelist-payload.json>
```

For the whitelist payload JSON consider the following:
- `<whitelist-payload.json>` uses the following format:
  ```
  '{"ip_ranges":["<ip_range_1>","<ip_range_2>",...,"<ip_range_n>"],"action":"<action>"}'
  ```

- `<ip_range>` must use the IPV4 CIDR format:
  
  For example:
  ```
  10.20.5.9/32, 10.50.0.0/16, ...
  ```
- `<action>` has only two valid values: `add` or `delete`

  `add` adds the specified IPs to an existing whitelist. `delete` removes the specified IPs from an existing whitelist.

  `<action>` is idempotent. If the same `<ip_range>` is specified more than once, it is added or deleted only once. Similarly. if  delete is invoked with an `<ip_range>` that doesn't exist in the current whitelist, it ìs  ignored.

  To remove all of the whitelisted IPs to the cluster, you need to invoke the `delete` action with all the `<ip_ranges>` that you added. To get the current whitelist, refer to [Retrieving the current whitelisted IP ranges](#retrieve-whitelist-IP-range).

A successful response of the private_endpoint_whitelist  API  endpoint is an updated list of the whitelisted `ip_ranges` in this format:
```
{ "private_endpoint_whitelist": ["<ip_range_1>", "<ip_range_2>"…/ip_range_n>"] }
```
The following example is a typical response of the API endpoint containing the present whitelist of IP ranges after an addition or deletion:
```
{"private_endpoint_whitelist": ["10.40.4.0/12", "10.50.5.0/19"]}
```
The following example is a response after deleting all of the IP ranges from the whitelist:
```
{ "private_endpoint_whitelist": []}
```

### Retrieving the current whitelisted IP ranges
{: #retrieve-whitelist-IP-range}

You can retrieve the current whitelisted IP ranges by invoking the `Get Details of Analytics Engine` API endpoint. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine) for more information. The parameter `private_endpoint_whitelist` has the list of the whitelisted IP ranges. Note that the `private_endpoint_whitelist` parameter is an optional field, which would be present only for a private endpoint enabled {{site.data.keyword.iae_full_notm}} instance. 

## Whitelisting access from the cluster

If you have a firewall or whitelisting mechanism at a destination, such as an on-premise service or other {{site.data.keyword.Bluemix_short}} services, such as PostgreSQL database or Object Storage, you can control the network traffic entering into the destination, from your {{site.data.keyword.iae_full_notm}} instance.

To do this, you need the list of all public or private IPs (depending on your network access method) that belong to your {{site.data.keyword.iae_full_notm}} instance so that you can add this list to the firewall rules at your destination.

You can get the list of IPs from the `Get Details of Analytics Engine` API. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine) for more information. The response JSON returns the `public_ip` and `private_ip` ranges of each node of the cluster.

