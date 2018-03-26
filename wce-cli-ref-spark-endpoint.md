---

copyright:
  years: 2017,2018
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# endpoint
## Description

Sets the {{site.data.keyword.iae_full_notm}} server endpoint.

**To create the ENDPOINT URI**
1. Get the hostname from [vcap](https://console.stage1.bluemix.net/docs/services/AnalyticsEngine/Retrieve-service-credentials-and-service-end-points.html#retrieve-service-credentials-and-service-end-points).

2. ENDPOINT_URI=https://hostname


## Usage

```
bx ae endpoint
      Displays current endpoint configuration

   bx ae endpoint ENDPOINT_URI
      Sets endpoint to provided value

   bx ae endpoint --unset
      Removes all endpoint information

   ENDPOINT_URI is the URI of the endpoint without a port number, e.g.) https://www.example.com
```

## Options

Flag    | Description
------- | -------------------------------
--unset | Remove all endpoint information

## Examples

### Set an endpoint

```
$ bx ae endpoint https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net
Registering endpoint 'https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net'...
Ambari Port Number [Optional: Press enter for default value] (9443)>
Knox Port Number [Optional: Press enter for default value] (8443)>
OK
Endpoint ''https://chs-xxx-xxx-mn001.bi.services.us-south.bluemix.net' set.
```

### Erase an endpoint

```
$ bx ae endpoint --unset
OK
Endpoint has been erased.
```
