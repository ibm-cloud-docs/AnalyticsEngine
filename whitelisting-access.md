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

# Whitelisting access to and from clusters
{: #whitelist-cluster-access}

You can use a whitelist to approve user access to and from  {{site.data.keyword.iae_full_notm}} clusters.

## Whitelisting incoming access to the cluster

You can whitelist user access to private endpoints or clusters that use {{site.data.keyword.Bluemix_notm}} service endpoints. See [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).

When you provision an {{site.data.keyword.iae_full_notm}} instance, you can choose if you want to access your cluster through the public internet or over the {{site.data.keyword.Bluemix_notm}} private network. If you choose to use private endpoints over the {{site.data.keyword.Bluemix_notm}} private network, you can choose to whitelist incoming access to the cluster by specifying a range of IP addresses.

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
  '{"ip_ranges":["<ip_range_1>","<ip_range_2>","<ip_range_3>",...,"<ip_range_n>"],"action":"<action>"}'
  ```

- `<ip_range>` must use the format:
  ```
  <ip_octate>.<ip_octate>.<ip_octate>.<ip_octate>/<cidr>
  ```
  For example:
  ```
  10.20.5.9/32, 10.50.0.0/16, ...
  ```
- `<action>` has only two valid values: `add` or `delete`

  `add` adds the specified IPs to an existing list. `delete` removes the specified IPs from an existing list.

  `<action>` is idempotent. If the same `<ip_range>` is specified more than once, it is added or deleted only once. Similarly. if  delete is invoked with an `<ip_range>` that doesn't exist, it ìs  ignored.

  To remove all of the whitelisted IPs to the cluster, you need to invoke the delete action with all the `<ip_ranges>` that you added.

The response of the invoked API is an updated list of the whitelisted `ip_ranges` in this format:
```
{ "private_endpoint_whitelist": ["<ip_range_1>", "<ip_range_2>", "<ip_range_3>", ..."<ip_range_n>"] }
```

For example:
```
{
	"private_endpoint_whitelist": ["10.40.4.0/12", "10.50.5.0/19"]
}
{
	"private_endpoint_whitelist": []
}
```

### Retrieving the current whitelisted IP ranges

You can retrieve the current whitelisted IP ranges by invoking the `Get Details of Analytics Engine` API. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine). The parameter `private_endpoint_whitelist` has the list of the whitelisted IP ranges.

## Whitelisting access from the cluster

You can whitelist user access to any of your on-prem resources or any services like {{site.data.keyword.cos_full_notm}} from the  {{site.data.keyword.iae_full_notm}} clusters.

To do this, you need the list of all public or private IPs (depending on your network access method) that belong to your {{site.data.keyword.iae_full_notm}} cluster so that you can add this list to the firewall rules on your on-prem resources or  services.

You can get the list of IPs from the `Get Details of Analytics Engine` API. See [Get details of Analytics Engine API](https://cloud.ibm.com/apidocs/ibm-analytics-engine#get-details-of-analytics-engine). The response JSON returns the `public_ip` and `private_ip` ranges of each node of the cluster.
