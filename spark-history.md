---

copyright:
  years: 2017, 2019
lastupdated: "2018-09-26"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Viewing Spark history
{: #spark-history}

Spark history provides access to job history information and various job metrics. You can also use the Spark history REST API. See the [Spark documentation](https://spark.apache.org/docs/latest/monitoring.html#rest-api) for information on the REST API.

## Opening and using Spark history

**To open Spark History**

1. Log in to Ambari.
2. Choose Spark2 service (left side).
3. Open Quick Links (top middle). If your  {{site.data.keyword.Bluemix_short}} hosting location is Dallas: `chs-XXXXX-mn002.<region>.ae.appdomain.cloud`
4. Choose Spark2 History Server UI.


** To use the Spark history REST API**

 The API is accessible via the Knox endpoint. For example, the call to retrieve the list of applications if your  {{site.data.keyword.Bluemix_short}} hosting location is Dallas:
```
curl -u "clsadmin:<password>" https://XXXXX-mn001.\
us-south.ae.appdomain.cloud:8443/gateway/default/sparkhistory/api/v1/applications```


**Note:** When you use the API with the YARN cluster mode, [app-id] will be [base-app-id]/[attempt-id], where [base-app-id] is the YARN application ID.
