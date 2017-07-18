---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# View Spark history 
The Spark History user interface provides access to job history information and various job metrics.

## To open the Spark History user interface
1. Log in to Ambari.
2. Choose Spark2 service (left side).
3. Open Quick Links (top middle).
4. Choose Spark2 History Server UI.

The Spark History appears in a browser page.

---

## To use Spark History REST API
Information on the REST API itself can be found in the Spark documentation
 * https://spark.apache.org/docs/latest/monitoring.html#rest-api 

The API is accessible via the Knox end point.  For example a call to retrieve the list of applications:
 * ```curl -k -u "iaeadmin:<password>" https://<host>:8443/gateway/sparkui/spark/sparkui/spark/api/v1/applications```


As noted in the Spark documentation when using the API with YARN cluster mode, [app-id] will actually be [base-app-id]/[attempt-id], where [base-app-id] is the YARN application ID.
