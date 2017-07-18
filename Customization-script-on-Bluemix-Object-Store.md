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

# Customization script on Bluemix Object Store

This page will guide you to
  * Upload your customization script and other associated artifacts (libraries/configurations etc) in [Bluemix Object Storage](https://console.ng.bluemix.net/catalog/services/object-storage)
  * Create a cluster with customization 
  * Download and use the artifacts in customization script

**Pre-reqs**

1. Bluemix object storage instance
 
Once you have the Bluemix object storage instance at “Service Credentials” tab, click the “View Credentials” drop down arrow. You will find the credentials json like below.  
   
     {
       "auth_url": "https://identity.open.softlayer.com",
       "project": "object_storage_xxxxxxxx_xxxx_xxxx_xxxx_xxxxxxxxxxxx",
       "projectId": "xxxxxxxxxxxxxxxxxxx",
       "region": "dallas",
       "userId": "xxxxxxxxxxxxxxxxxxxxx",
       "username": "admin_xxxxxxxxxxxxxxxxxxxxx",
       "password": "xxxxxxxxxxxxxxx",
       "domainId": "xxxxxxxxxxxxxxxxxxxxxxxxxxx",
       "domainName": "899849",
       "role": "admin"
     }


**How to upload the artifacts in Bluemix object storage?**

  * Create a container in object store, if not yet created

 On the bluemix object store manage tab, click on the "Select Actions" drop down and select "Create Container" action. In the dialog box, provide a name (myContainer) and save. 

  * Upload your bootstrap script and libraries into container.
   
 On the newly created container page, Click on "Select Actions" drop down and select "Add Files" action. It will allow you to upload your files from your system. We uploaded analytic-libs.tgz file, which contains all our required libraries. 

**How to consume the artifacts from Bluemix object storage in your customization script?**

You have just uploaded the required libraries (analytic-libs.tgz) in Bluemix object store. Now, you have to write a customization script, which can download the above library files and perform some action using them.

Here is a sample customization script

 ``` 
#!/bin/bash
echo "executing customization script..."
mkdir -p /tmp/analytic-libs
curl --header "X-Auth-Token:[AUTH_TOKEN]" -o /tmp/analytic-libs/analytic-libs.tgz https://dal.objectstorage.open.softlayer.com/v1/AUTH_[PROJECT_ID]/myContainer/analytic-libs.tgz

#untar file into common location for scala and pyton  
tar -xvzf /tmp/analytic-libs/analytic-libs.tgz -C /home/common/lib/scala/spark16
rm -rf /tmp/analytic-libs
echo "downloaded and extracted Analytic-libs"
# Do some thing more here
echo "executed custom script successfully."

  ```

**Note**: You need to fill in the following details in the above script

* [PROJECT_ID] = projectId from your object store credentials json
* [AUTH_TOKEN] = authentication token for bluemix object storage

To generate the AUTH_TOKEN execute the following command

```
   curl -D Auth_Token.txt -X POST -H "Content-Type: application/json" -d '{ "auth":{ "identity":{ "methods": ["password"], "password":{"user":{"id": "[userId from credentials json]", "password": "[password from credentials json]"} }}, "scope":{ "project":{"id": "[projectId from credentials josn]"}}}}' 'https://identity.open.softlayer.com/v3/auth/tokens'

```

Pick the  X-Auth-Token from Auth_Token.txt file and update the script above.

Your customization script is ready. Upload the script(bootstrap.sh) in the same bluemix object store.


**How to execute your customization script?**

Create a cluster with a customization action pointing to the above script file. Use the following json as input to the create cluster request. 
```
{
        "num_compute_nodes": 1,
        "hardware_config": "Standard",
        "software_package": "iae-0.1-SparkPack",
        "customization":[ 
               {
   		"name": "sample",
    		"type": "bootstrap",
    		"script": 
		   {
        		"source_type": "BluemixSwift",
        		"source_props": {
           		    "authUrl": "https://identity.open.softlayer.com",
           		    "userId": "xxxxxxxxxxxxxxxxxxxxx",
           		    "password": "xxxxxxxxxxxxxxx",
           		    "projectId": "xxxxxxxxxxxxxxxxxxx",
           		    "region": "dallas"
        	    },
         	"script_path": "/myContainer/bootstrap.sh"
   	 	},
    		"scriptParams": []
 	     }]

}

```

More details on execution of create cluster  [here](./provisioning.html#creating-a-service-instance).

**How to validate the customization?**
  
Details [here](./customizing-cluster.html#using-cluster-management-rest-api-to-get-cluster-customization-status).


**How to rerun your customization script?**

If some thing fails, you can rerun the customization script on selected nodes using the below command.

```
curl -X POST -v "https://ibmae-api.ng.bluemix.net/v2/analytics_engines/<service_instance_id>/customization_requests" -d '{"target":"all"}'  -H "Api-Auth:<instance_id> <api_key>" -H "Content-Type: application/json"

```
Specify the target value as per your requirement. Possible values 

* all  : run on all nodes
* management : run only on master management node
* data : run on all data nodes
* comma separated list of hostnames (fully qualified hostname) : to run only on those nodes
