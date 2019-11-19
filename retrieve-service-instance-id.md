---

copyright:
  years: 2017, 2019
lastupdated: "2018-11-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Retrieving the service instance ID
{: #retrieve-service-id}

Before you can retrieve the service instance ID, ensure that you have the following prerequisites:
- You must have a valid IBMid.
- You must have installed the {{site.data.keyword.Bluemix_notm}} CLI.

To retrieve the service instance ID:

1. Log in to the {{site.data.keyword.Bluemix_notm}} CLI:
```
ibmcloud api https://cloud.ibm.com
ibmcloud login <enter your credentials>
```
If you share an {{site.data.keyword.Bluemix_notm}} account, you'll be asked to choose an account for the current session.
2. List all service instances:
```
ibmcloud resource service-instances
```
Sample response:
```
Retrieving service instances in resource group Default and all locations under account ...
OK
Name                                     Location   State    Type              Tags
MyServiceInstance                        us-south   active   service_instance
```
3. Retrieve service instance ID:
```
ibmcloud resource service-instance MyServiceInstance --id
```
Sample response:
```
crn:v1:yp:public:rc-dev-siba1:us-south:a/217b381692bae8d7153823013f2cacf8:046a1611-81ff-4215-a5e0-6af8d7910610::
```
In the sample response, `046a1611-81ff-4215-a5e0-6af8d7910610` is the service instance ID.
