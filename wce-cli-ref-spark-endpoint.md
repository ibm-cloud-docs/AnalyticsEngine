---

copyright:
  years: 2017,2018
lastupdated: "2018-09-17"

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
1. Get the hostname from [vcap](./Retrieve-service-credentials-and-service-end-points.html).

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

The following command shows how to set the endpoint if your {{site.data.keyword.Bluemix_short}} hosting location is `us-south`.

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
