---

copyright:
  years: 2017, 2019
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Troubleshooting (JNBG)
{: #Troubleshooting-JNBG}

## Common Issues

**Executing code in a notebook cell displays a message “Waiting for a Spark session to start...” or “Obtaining Spark session...” and the kernel indicator remains in a busy state (filled circle) for a very long time (over a minute).**

The most likely cause is that you don't have sufficient YARN containers available on the cluster to initiate a new Spark session. The notebook request is waiting for YARN resources to free up. You can verify this by checking that the state of your application reported by YARN is 'ACCEPTED'. You can access YARN via its Resource Manager web interface (accessible using the cluster’s Ambari console) or using its command line utility (accessible once you SSH to the cluster).

To fix this issue, free up resources by shutting down any other running notebooks, interactive sessions (via JNBG, Livy etc.) or YARN applications.
