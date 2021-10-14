---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Resetting cluster password
{: #reset-cluster-password}

To reset an {{site.data.keyword.iae_full_notm}} cluster password, you must have the following [user permissions](/docs/AnalyticsEngine?topic=AnalyticsEngine-grant-permissions).

You can reset a cluster’s password by using the {{site.data.keyword.iae_full_notm}} REST API. This API
resets the cluster's password to a new crytographically strong value. Note that you must replace all existing service keys and rebind all bound applications after the password is reset.

To reset the cluster's password by using the {{site.data.keyword.iae_full_notm}} REST API, enter the
following cURL command:  
```sh
curl -X  POST  \
https://api.us-south.ae.cloud.ibm.com/v2/analytics_engines/<service_instance_guid>/reset_password
\ -H 'authorization: Bearer  <user's IAM token>'
```

For the United Kingdom region, use the endpoint `https://api.eu-gb.ae.cloud.ibm.com`. For Germany, use `https://api.eu-de.ae.cloud.ibm.com` and for Japan `https://api.jp-tok.ae.cloud.ibm.com`.

The expected response is the changed password in JSON format. For example:
```json
{"id":"5259c951-689a-4eac-a48e-0ae22b45b786","user_credentials":{"user":"clsadmin","password":"modifiedpassword"}}
```

**Note**: To retrieve the IAM access token, see [Retrieving IAM access tokens](/docs/AnalyticsEngine?topic=AnalyticsEngine-retrieve-iam-token).
