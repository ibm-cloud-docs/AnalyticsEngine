---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-15"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Using an allowlist to control network traffic
{: #allowlist-to-cluster-access}

You can control network traffic to your {{site.data.keyword.iae_full_notm}} instances based on where the network traffic (IP address) originates. Requests that do not originate from IP addresses in the allowlist are denied access.

You can only use an allowlist for privately enabled endpoints to an {{site.data.keyword.iae_full_notm}} instance.

## Controlling network traffic into the instance

When you provision an {{site.data.keyword.iae_full_notm}} instance, you can choose to create it with either public or private endpoints. If you use [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration), you can optionally add IP address ranges to an allowlist to control incoming network traffic to your instance.

### Updating IP ranges in the allowlist

You can add or delete IP ranges to and from the allowlist of an {{site.data.keyword.iae_full_notm}} instance by invoking the `PATCH` operation on the `private_endpoint_allowlist` endpoint of the {{site.data.keyword.iae_full_notm}} cluster management API.

For example, if you created your service instance in the `US-South` {{site.data.keyword.Bluemix_notm}} region, enter:
```sh
curl --request PATCH  https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/ <service_instance_guid>/private_endpoint_allowlist
\ -H "Authorization: Bearer <user's IAM token>"
\ -H "accept: application/json"
\ -H "Content-type: application/json"
\ -d <allowlist-payload.json>
```
{: codeblock}


For the allowlist payload JSON consider the following:
- `<allowlist-payload.json>` uses the following format:
    ```json
    {
      "ip_ranges": ["<ip_range_1>", "<ip_range_2>", ..., "<ip_range_n>"],
      "action": "<action>"
    }
    ```
Where:

- `<ip_range>` must use the IPV4 CIDR format:

    For example:
    ```text
    10.20.5.9/32, 10.50.0.0/16, ...
    ```
- `<action>` has only two valid values: `add` or `delete`

    `add` adds the specified IPs to an existing allowlist. `delete` removes the specified IPs from an existing allowlist.

    `<action>` is idempotent. If the same `<ip_range>` is specified more than once, it is added or deleted only once. Similarly, if `delete` is invoked with an `<ip_range>` that doesn't exist in the current allowlist, it ìs ignored.

    To remove all of the IPs to the cluster in the allowlist, you need to invoke the `delete` action with all the `<ip_ranges>` that you added. To get the current allowlist, refer to [Retrieving the current IP ranges in the allowlist](#retrieve-ip-range).

A successful response of the `private_endpoint_allowlist` API endpoint is an updated list of the `ip_ranges` in the allowlist in this format:
```json
{ "private_endpoint_allowlist": ["<ip_range_1>", "<ip_range_2>"…/ip_range_n>"] }
```
The following example is a typical response of the API endpoint containing the present allowlist of IP ranges after an addition or deletion:
```json
{"private_endpoint_allowlist": ["10.40.4.0/12", "10.50.5.0/19"]}
```
The following example is a response after deleting all of the IP ranges from the allowlist:
```json
{ "private_endpoint_allowlist": []}
```

### Retrieving the current IP ranges in the allowlist
{: #retrieve-ip-range}

You can retrieve the current IP ranges in the allowlist by invoking the `Get Details of Analytics Engine` API endpoint. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine) for more information. The parameter `private_endpoint_allowlist` has the list of the IP ranges in the allowlist. Note that the `private_endpoint_allowlist` parameter is an optional field, which would be present only for a privately enabled endpoint to an  {{site.data.keyword.iae_full_notm}} instance.

## Controlling network traffic from Analytics Engine IP addresses at a destination

If you have a firewall or are using an allowlisting mechanism at a destination, such as an on-premise service or other {{site.data.keyword.Bluemix_short}} services like a PostgreSQL database or Object Storage, you can control the network traffic entering into the destination, from your {{site.data.keyword.iae_full_notm}} instance.

To do this, you need the list of all public or private IPs (depending on your network access method) that belong to your {{site.data.keyword.iae_full_notm}} instance so that you can add this list to the firewall rules at your destination.

You can get the list of IPs from the `Get Details of Analytics Engine` API. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine) for more information. The response JSON returns the `public_ip` and `private_ip` ranges of each node of the cluster.
