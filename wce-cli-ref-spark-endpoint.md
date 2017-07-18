---

copyright:
  years: 2017
lastupdated: "2017-07-13"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# spark‐endpoint
## Description

Sets the IBM Analytics Engine server endpoint.

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
$ bx ae endpoint https://169.54.195.210
Registering endpoint 'https://169.54.195.210'...
Ambari Port Number [Optional: Press enter for default value] (9443)>
Knox Port Number [Optional: Press enter for default value] (8443)>
OK
Endpoint 'https://169.54.195.210' set.
```

### Erase an endpoint

```
$ bx ae endpoint --unset
OK
Endpoint has been erased.
```
