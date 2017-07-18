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

# Accessing the Jupyter Kernel Gateway service

## Jupyter Kernel Gateway (JNBG) service endpoint

Jupyter Kernel Gateway clients access:

1. [HTTP resources](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html#http-resources) of the Jupyter Kernel Gateway API for operations like retrieving kernel specifications, listing running kernels, and starting, stopping and deleting kernels. 

2. [Websocket resources](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html#websocket-resources) of the Jupyter Kernel Gateway API connection to a running kernel to submit code and communicate with the kernel. 

The JNBG service on the cluster makes available two endpoints - one for HTTP operations and another for Websocket. 

Refer to the instructions [here](./retrieving-credentials.html) on retrieving service end points for the IBM Analytics Engine cluster. In the JSON service endpoint details, the HTTP endpoint URL of the JNBG service is listed in `notebook_gateway` and the Websocket endpoint in `notebook_gateway_websocket`. Here is a representative sample of a cluster's service endpoint details:

```
.
.
      "cluster": {
        "cluster_id": "20170412-084729-981-mzVunjuU",
        "user": "iaeadmin",
        "password": "5auuF5SU3e0G",
        "password_expiry_date": "null",
        "service_endpoints": {
          "ambari_console": "https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:9443",
          "notebook_gateway": "https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/",
          "notebook_gateway_websocket": "wss://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/",
          "webhdfs": "https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/webhdfs/v1/",
          "ssh": "ssh iaeadmin@chs-zbh-288-mn003.bi.services.us-south.bluemix.net",
          "livy": "https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/livy/v1/batches"
        }
      }
.
.
```

In this sample,

* the JNBG HTTP REST API is accessible on the `https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/` endpoint and, 
* Websocket calls can be made on `wss://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/` endpoint

## Authentication

Access to the JNBG service endpoints is SSL secured and requires `BASIC` authentication. Use the `user` and `password` included in the cluster's json service endpoint to add the `BASIC` authorization header in your HTTP and Websocket connection calls to the JNBG service.

## Example configuration: notebook server with `nb2kg` extension

Typically, Jupyter Notebook servers use the `nb2kg` extension to connect with remote kernel gateways such as JNBG. 

The IBM Open Platform provides an updated `nb2kg` package [here](http://ibm-open-platform.ibm.com:8080/simple/nb2kg/) which is modified to accept additional endpoint and authentication details needed to access a Jupyter Kernel Gateway instance requiring such authentication. When using this version of `nb2kg`, the following configuration is needed to access the cluster's JNBG service:

* configure the `KG_URL` to the HTTP endpoint URL of the JKG service
* configure the `KG_WS_URL` to the Websocket endpoint URL of the JKG service
* configure the `KG_HTTP_USER` to the cluster user
* configure the `KG_HTTP_PASS` to the cluster password

As per the sample IBM Analytics Engine cluster response details above, the configuration would be:

```
KG_URL=https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/
KG_WS_URL=wss://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/
KG_HTTP_USER=iaeadmin
KG_HTTP_PASS=5auuF5SU3e0G
```

## REST API access

The JNBG service exposes the Jupyter Kernel Gateway REST API which can be used by any remote interactive client to launch kernels and submit code for execution to them. 

Here are some commonly used REST APIs:

| Method | Endpoint | Description |
|---------|------------|-----------|
| GET | /api | Get API info (e.g. returns {"version": "4.3.1"}) |
| GET | /api/kernelspecs | Get kernel specs which is useful to know during kernel creation  |
| GET |  /api/kernels | List Kernels |
| POST |  /api/kernels | Start a kernel and return the uuid |
| GET | /api/kernels/{kernel_id} | Get kernel information |
| DELETE | /api/kernels/{kernel_id} | Kill a kernel and delete the kernel id |
| POST | /kernels/{kernel_id}/interrupt |  interrupt a kernel |
| POST | /kernels/{kernel_id}/restart | restarts a kernel |

For complete details about the API refer the documentation and swagger specifications provided [here](http://jupyter-kernel-gateway.readthedocs.io/en/latest/websocket-mode.html).

## Examples  

**Example 1: Creating an Spark application using the IBM Analytics Engine Interactive API**

This sample application creates the Spark kernel using the IBM Analytics Engine Interactive API service and runs simple Spark code against the kernel.

To create a sample application that runs on a Linux system:

1- Prepare the environment in which you run the sample application. Run the following commands to install the required Node packages:

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
npm install
```

2- Create the sample application file. Create a file named spark-interactive-demo.js and copy the following content to the file. Adjust the `notebook_gateway` host variable values to use the VCAP credentials of the IBM Analytics Engine service instance that you have created.

```
// Access variables with the notebook_gateway VCAP information from your IBM Analytics Engine service.
var notebook_gateway = 'https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg/';
var notebook_gateway_ws = 'wss://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws/';

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

// Start a kernel.
jupyter.startNewKernel({
    baseUrl: notebook_gateway,
    wsUrl:' notebook_gateway_ws,
    name:'python2-spark21'
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
For more information on jupyter-js-services, see https://github.com/jupyterlab/services.

3- Run the sample application. Enter the following command to run the Node client:

```
node ~/spark-example/spark-interactive-demo.js
```

The sample Python code sourceToExecute that runs against the Spark kernel takes 5 numbers and then displays them in the JSON output. Example:

```
content: { text: '[882, 635, 978, 219, 773]\n', name: 'stdout' },
```

**Example 2 - Using the Jupyter Kernel Gateway provided code samples in https://github.com/jupyter/kernel_gateway_demos.**

a- Run the following command to get the demo programs:

```
git clone https://github.com/jupyter/kernel_gateway_demos.git ~/kernel_gateway_demos
```

b- Run the following commands substituting the correct HTTP and Websocket URLs for your cluster:

```
export BASE_GATEWAY_HTTP_URL=https://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkg
export BASE_GATEWAY_WS_URL=wss://chs-zbh-288-mn001.bi.services.us-south.bluemix.net:8443/gateway/default/jkgws
```

c- Modify the `user` and `password` values in the `client.py` file to the correct values for your cluster

d- Run the demo Python client:

```
cd ~/kernel_gateway_demos/python_client_example/src python client.py --lang="python2-spark21"
```
