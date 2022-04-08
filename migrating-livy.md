---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-01"

subcollection: analyticsengine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:external: target="_blank" .external}

# Migrating from classic instances: livy API
{: #migrate-livy}

This topic helps you when migrating from classic to serverless instances by providing a brief summary of the differences and sample code snippets when using the Livy API to submit and manage batch jobs in {{site.data.keyword.iae_short}}.

## Using the spark-submit API

The following table summarizes the differences between classic and serverless instances when you use the Livy API.

| Classic instances     | Serverless instances |
|-----------------------|--------------------------|
| - Based on Apache Livy v1   \n - Supports basic authentication requiring user and password   \n - Endpoint is routed through Apache Knox   \n - Requires the header `X-Requested-By: livy`   \n - You need to specify `proxyUser` in the payload | - Livy like API   \n - Supports token based authentication   \n - Endpoint goes against the instance |

## Livy batch API examples

The following examples show the code for classic instances with the corresponding equivalent for serverless instances.

- Submit a job using the Livy API endpoint

    Sample of the command for classic instances:
    ```
    curl \
    -u "clsadmin:itissecret" \
    -H 'Content-Type: application/json' \
    -H 'X-Requested-By: livy'  \
    -d '{ "file":"cos://mybucket.myprodservice/PiEx.py", "proxyUser":"clsadmin" }' \
    "https://chs-tmp-867-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches"
    ```
    Corresponding command for serverless instances:
    ```
    curl \
    -H 'Authorization: Bearer <TOKEN>' \
    -H 'Content-Type: application/json' \
    -d '{ "file": "cos://mybucket.myprodservice/PiEx.py"}'\
    -X POST "https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches"
    ```
- Get job details using the Livy API endpoint

    Sample of the command for classic instances:
    ```
    curl -s -i \
    -u "clsadmin:itissecret" \
    https://chs-tmp-867-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches/34
    ```
    Corresponding command for serverless instances:
    ```
    curl -H 'Authorization: Bearer <TOKEN>' \ 
    -H 'Content-Type: application/json' \
    -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/34
    ```
- Get the state of a job using the Livy API endpoint

    Sample of the command for classic instances:
    ```
    curl -s -i \
    -u "<username>:<password>" \
    https://chs-tmp-867-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches/34/state
    ```
    Corresponding command for serverless instances:
    ```
    curl -H 'Authorization: Bearer <TOKEN>' \ 
    -H 'Content-Type: application/json' \
    -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/34/state
    ```
- Get all jobs using the Livy API endpoint

    Sample of the command for classic instances:
    ```
    curl -s -i \
    -u "clsadmin:itissecret" \
    https://chs-tmp-867-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches/
    ```
    Corresponding command for serverless instances:
    ```
    curl -H 'Authorization: Bearer <TOKEN>' \
    -H 'Content-Type: application/json' \
    -X GET https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches
    ```    
- Delete jobs using the Livy API endpoint

    Sample of the command for classic instances:
    ```
    curl -s -i \ 
    -u "<username>:<password>" \ 
    -H 'X-Requested-By: livy'  \ 
    -X DELETE \ https://chs-tmp-867-mn001.us-south.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches/34
    ```
    Corresponding command for serverless instances:
    ```
    curl -H 'Authorization: Bearer <TOKEN>' \ 
    -H 'Content-Type: application/json' \
    -X DELETE https://api.us-south.ae.cloud.ibm.com/v3/analytics_engines/<instance-id>/livy/batches/<batch-id>
    ```        