---

copyright:
  years: 2017,2018
lastupdated: "2018-09-27"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Accessing the Jupyter Notebook Gateway service

## Jupyter Notebook Gateway (JNBG) service endpoint

The JNBG service on the cluster provides two endpoints for HTTP operations and Websocket resource.

* [HTTP resources](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html#http-resources)

 The HTTP API consists of resources for operations like retrieving kernel specifications, listing running kernels, and starting, stopping, and deleting kernels.

* [Websocket resource](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html#websocket-resources)

 The Websocket resource multiplexes the Jupyter kernel messaging protocol over a single Websocket connection to submit code and communicate with the running kernel.

Refer to the instructions [here](./Retrieve-service-credentials-and-service-end-points.html#retrieving-service-credentials-and-service-end-points) on retrieving service end points for the {{site.data.keyword.iae_full_notm}} cluster. In the JSON service endpoint details, the HTTP endpoint URL of the JNBG service is listed in `notebook_gateway` and the Websocket endpoint in `notebook_gateway_websocket`. Here is a representative sample of a cluster's service endpoint details:

```
.
.
      "cluster": {
        "cluster_id": "20170412-084729-981-mzVunjuU",
        "user": "clsadmin",
        "password": "5auuF5SU3e0G",
        "password_expiry_date": "null",
        "service_endpoints": {
          "ambari_console": "https://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:9443",
          "notebook_gateway": "https://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkg/",
          "notebook_gateway_websocket": "wss://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkgws/",
          "webhdfs": "https://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/webhdfs/v1/",
          "ssh": "ssh clsadmin@chs-zbh-288-mn003.<changeme>.ae.appdomain.cloud",
          "livy": "https://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/livy/v1/batches"
        }
      }
.
.
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

In this sample, notice the following information:

* The JNBG HTTP REST API is accessible on the `https://chs-zbh-288-mn001.<changme>.ae.appdomain.cloud:8443/gateway/default/jkg/` endpoint and,
* Websocket calls can be made on the  `wss://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkgws/` endpoint.

## Authentication

Access to the JNBG service endpoints is SSL secured and requires `BASIC` authentication. Use the `user` and `password` included in the cluster's JSON service endpoint to add the `BASIC` authentication header in your HTTP and Websocket connection calls to the JNBG service.

## Example configuration: notebook server with `nb2kg` extension

Typically, Jupyter Notebook servers use the `nb2kg` extension to connect with remote kernel gateways such as JNBG.

The IBM Open Platform provides an updated `nb2kg` package [here](http://ibm-open-platform.ibm.com:8080/simple/nb2kg/) which is modified to accept additional endpoint and authentication details needed to access a Jupyter Kernel Gateway instance requiring authentication. When using this version of `nb2kg`, the following configuration is needed to access the cluster's JNBG service:

* Configure the `KG_WS_URL` to the Websocket endpoint URL of the JKG service
* Configure the `KG_HTTP_USER` to the cluster user
* Configure the `KG_HTTP_PASS` to the cluster password

For the previous  {{site.data.keyword.iae_full_notm}} cluster response details, the configuration would be:

```
KG_URL=https://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkg/
KG_WS_URL=wss://chs-zbh-288-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkgws/
KG_HTTP_USER=clsadmin
KG_HTTP_PASS=5auuF5SU3e0G
```
where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

## REST API access

The JNBG service exposes the Jupyter Kernel Gateway REST API which can be used by any remote interactive client to launch kernels and submit code for execution to them.

Here are some commonly used REST APIs:

| Method | Endpoint | Description |
|---------|------------|-----------|
| GET | /api | Get API info (For example, returns `{"version": "4.3.1"}`) |
| GET | /api/kernelspecs | Gets kernel specs which is useful to know during kernel creation  |
| GET |  /api/kernels | Lists kernels |
| POST |  /api/kernels | Starts a kernel and return the UUID |
| GET | /api/kernels/{kernel_id} | Gets kernel information |
| DELETE | /api/kernels/{kernel_id} | Kills a kernel and delete the kernel ID |
| POST | /kernels/{kernel_id}/interrupt | Interrupts a kernel |
| POST | /kernels/{kernel_id}/restart | Restarts a kernel |

For complete details about the API refer the documentation and swagger specifications provided [here](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html).

## Examples

Because the Jupyter Kernel Gateway service exposes an HTTP- and WebSocket-based API, Spark interactive applications can be written in any language of choice by referencing the API specifications and the [Jupyter Wire Protocol description](https://jupyter-client.readthedocs.io/en/stable/messaging.html).

Refer to the following sample applications written for Node.js and Python 2.

**Example 1: Creating an Node.js Spark application using the IBM Analytics Engine interactive API**

This sample application creates the Spark kernel using the IBM Analytics Engine interactive API service and runs Spark code against the kernel.

**To create a sample application that runs on a Linux system:**

1. Prepare the environment in which you run the sample application. Run the following commands to install the required Node packages:
```
mkdir ~/spark-example
cat <<EOT > ~/spark-example/package.json
{
  "name": "spark-example",
  "version": "0.0.0",
  "private": true,
  "dependencies": {
    "jupyter-js-services": "^0.9.0",
    "ws": "^0.8.0",
    "xmlhttprequest": "^1.8.0"
  }
}
EOT
cd ~/spark-example
yum install -y epel-release nodejs npm; npm install
```
2. Create the sample application file. Create a file named spark-interactive-demo.js and copy the following content to the file. Adjust the `notebook_gateway` and `notebook_gateway_ws`  host variable values to use the VCAP credentials of the IBM Analytics Engine service instance that you have created.

  For authentication, set the environment variables: BASE_GATEWAY_USERNAME and BASE_GATEWAY_PASSWORD. Fetch the username and password values from VCAP.
```
// Access variables with the notebook_gateway VCAP information from your IBM Analytics Engine service.
var notebook_gateway = 'https://chs-zys-882-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkg/';
var notebook_gateway_ws = 'wss://chs-zys-882-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkgws/';
// Client program variables.
var xmlhttprequest =require('xmlhttprequest');
var ws =require('ws');
global.XMLHttpRequest=xmlhttprequest.XMLHttpRequest;
global.WebSocket= ws;
var jupyter =require('jupyter-js-services');
// Sample source code to run against the Spark kernel.
var sourceToExecute =`
import pyspark
rdd = sc.parallelize(range(1000))
sample = rdd.takeSample(False, 5)
print(sample)`
var ajaxSettings = {};
// For authentication, set the environment variables:
// BASE_GATEWAY_USERNAME and BASE_GATEWAY_PASSWORD.
if (process.env.BASE_GATEWAY_USERNAME) {
    ajaxSettings['user'] = process.env.BASE_GATEWAY_USERNAME
}
if (process.env.BASE_GATEWAY_PASSWORD) {
    ajaxSettings['password'] = process.env.BASE_GATEWAY_PASSWORD
}
// Start a kernel.
jupyter.startNewKernel({
    baseUrl: notebook_gateway,
    wsUrl: notebook_gateway_ws,
    name: 'python2-spark21',
    ajaxSettings: ajaxSettings
})
// Run the sample source code against the kernel.
.then((kernel) => {
    var future =kernel.execute({ code: sourceToExecute } );
    future.onDone= () => { process.exit(0); };
    future.onIOPub= (msg) => { console.log('Received message:', msg); };
}).catch(req=> {
    console.log('Error starting new kernel:', req.xhr.statusText);
    process.exit(1);
});
```
 where `<changeme>` is the {{site.data.keyword.Bluemix_short}} hosting location, for example `us-south`.

 For more information on jupyter-js-services, see https://github.com/jupyterlab/services.

3. Run the sample application. Enter the following command to run the Node client:
```
node ~/spark-example/spark-interactive-demo.js
```

  The sample Python code `sourceToExecute` that runs against the Spark kernel takes five numbers and then displays them in the JSON output. Example:
```
content: { text: '[882, 635, 978, 219, 773]\n', name: 'stdout' },
```

**Example 2: Creating a Python 2 Spark application using the IBM Analytics Engine Interactive API**

This Python 2 sample code uses Tornado libraries to make HTTP and WebSocket calls to a Jupyter Kernel Gateway service. You need a Python 2 runtime environment with the Tornado package installed to run this sample code.

**To create a Python 2 Spark application:**

In any working directory create a file `client.py` containing the following code:
```
from uuid import uuid4

from tornado import gen
from tornado.escape import json_encode, json_decode, url_escape
from tornado.httpclient import AsyncHTTPClient, HTTPRequest
from tornado.ioloop import IOLoop
from tornado.websocket import websocket_connect

@gen.coroutine
def main():
    kg_http_url = "https://chs-xxx-yyy-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkg/"
    kg_ws_url = "wss://chs-xxx-yyy-mn001.<changeme>.ae.appdomain.cloud:8443/gateway/default/jkgws/"
    auth_username = 'clsadmin'
    auth_password = '1qazxsw23edc'
    validate_cert = True

    kernel_name = "scala-spark21"
    code = """
        print(s"Spark Version: ${sc.version}")
        print(s"Application Name: ${sc.appName}")
        print(s"Application ID: ${sc.applicationId}")
        print(sc.parallelize(1 to 5).count())
        """

    print("Using kernel gateway URL: {}".format(kg_http_url))
    print("Using kernel websocket URL: {}".format(kg_ws_url))

    # Remove "/" if exists in JKG url's
    if kg_http_url.endswith("/"):
        kg_http_url=kg_http_url.rstrip('/')
    if kg_ws_url.endswith("/"):
        kg_ws_url=kg_ws_url.rstrip('/')

    client = AsyncHTTPClient()

    # Create kernel
    # POST /api/kernels
    print("Creating kernel {}...".format(kernel_name))
    response = yield client.fetch(
        '{}/api/kernels'.format(kg_http_url),
        method='POST',
        auth_username=auth_username,
        auth_password=auth_password,
        validate_cert=validate_cert,
        body=json_encode({'name': kernel_name})
    )
    kernel = json_decode(response.body)
    kernel_id = kernel['id']
    print("Created kernel {0}.".format(kernel_id))

    # Connect to kernel websocket
    # GET /api/kernels/<kernel-id>/channels
    # Upgrade: websocket
    # Connection: Upgrade
    print("Connecting to kernel websocket...")
    ws_req = HTTPRequest(url='{}/api/kernels/{}/channels'.format(
        kg_ws_url,
        url_escape(kernel_id)
    ),
        auth_username=auth_username,
        auth_password=auth_password,
        validate_cert=validate_cert
    )
    ws = yield websocket_connect(ws_req)
    print("Connected to kernel websocket.")

    # Submit code to websocket on the 'shell' channel
    print("Submitting code: \n{}\n".format(code))
    msg_id = uuid4().hex
    req = json_encode({
        'header': {
            'username': '',
            'version': '5.0',
            'session': '',
            'msg_id': msg_id,
            'msg_type': 'execute_request'
        },
        'parent_header': {},
        'channel': 'shell',
        'content': {
            'code': code,
            'silent': False,
            'store_history': False,
            'user_expressions': {},
            'allow_stdin': False
        },
        'metadata': {},
        'buffers': {}
    })
    # Send an execute request
    ws.write_message(req)

    print("Code submitted. Waiting for response...")
    # Read websocket output until kernel status for this request becomes 'idle'
    kernel_idle = False
    while not kernel_idle:
        msg = yield ws.read_message()
        msg = json_decode(msg)
        msg_type = msg['msg_type']
        print ("Received message type: {}".format(msg_type))

        if msg_type == 'error':
            print('ERROR')
            print(msg)
            break

        # evaluate messages that correspond to our request
        if 'msg_id' in msg['parent_header'] and \
                        msg['parent_header']['msg_id'] == msg_id:
            if msg_type == 'stream':
                print("  Content: {}".format(msg['content']['text']))
            elif msg_type == 'status' and \
                            msg['content']['execution_state'] == 'idle':
                kernel_idle = True

    # close websocket
    ws.close()

    # Delete kernel
    # DELETE /api/kernels/<kernel-id>
    print("Deleting kernel...")
    yield client.fetch(
        '{}/api/kernels/{}'.format(kg_http_url, kernel_id),
        method='DELETE',
        auth_username=auth_username,
        auth_password=auth_password,
        validate_cert=validate_cert
    )
    print("Deleted kernel {0}.".format(kernel_id))


if __name__ == '__main__':
    IOLoop.current().run_sync(main)

```
{: codeblock}

Update `client.py` using your cluster VCAP:

* Set the `kg_ws_url` variable to the notebook_gateway_websocket value in your cluster VCAP.
* Set the `auth_username` variable to the user value in your cluster VCAP.
* Set the `auth_password` variable to the password value in your cluster VCAP.

Install pip. If you don't have it, install the Python Tornado package using this command:
```
yum install –y python-pip; pip install tornado.
```
{: codeblock}

Run the demo Python client:
```
python client.py
```

{: codeblock}

The code above creates a Spark 2.1 Scala kernel and submits Scala code to it for execution.

Here are code snippets to show how the kernel name and code variables can be modified in `client.py` to do the same for Python 2 and R kernels.

* Python 2
```
kernel_name = "python2-spark21"
code = '\n'.join(( "print(\"Spark Version: {}\".format(sc.version))", "print(\"Application Name: {}\".format(sc._jsc.sc().appName()))", "print(\"Application ID: {} \".format(sc._jsc.sc().applicationId()))", "sc.parallelize([1,2,3,4,5]).count()" ))
```

 {: codeblock}

* R
```
kernel_name = "r-spark21"
code = """
 cat("Spark Version: ", sparkR.version())
 conf = sparkR.callJMethod(spark, "conf")
 cat("Application Name: ", sparkR.callJMethod(conf, "get", "spark.app.name"))
 cat("Application ID:", sparkR.callJMethod(conf, "get", "spark.app.id"))
 df <- as.DataFrame(list(1,2,3,4,5))
 cat(count(df))
 """
```
{: codeblock}
