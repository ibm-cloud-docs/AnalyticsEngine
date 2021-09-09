---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-09"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Limits and quotas for {{site.data.keyword.iae_short}} instances
{: #limits}

The following sections provide technical details about the limitation and quota settings for {{site.data.keyword.iae_full_notm}} serverless instances.
{: shortdesc}

## Application limits
{: #limits_application}

The following table lists the default limits and quotas for {{site.data.keyword.iae_short}} instances.


| Category                                |        Default         |
| --------------------------------------- | ---------------------- |
| Maximum number of instances per account |                      5 |
| Maximum cores per instance              |                    100 |
| Maximum memory per instances            |                 400 GB |
| Shuffle space per core                  |           approx. 8 GB |
{: caption="Instance limits"}


## Supported Spark driver and executor vCPU and memory combinations
{: #cpu-mem-combination}

The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark  supports only the following pre-defined Spark driver and executor vCPU and memory combinations.

These two vCPU to memory proportions are supported: 1 vCPU to 4 GB of memory and 1 vCPU to 8 GB of memory.

The following table shows the supported vCPU to memory size combinations.

| vCPU to memory combinations |
| --------------------------- |
| 1 vCPU x 4 GB |
| 2 vCPU x 8 GB |
| 3 vCPU x 12 GB |
| 4 vCPU x 16 GB|
| 5 vCPU x 20 GB |
| 6 vCPU x 24 GB |
| 1 vCPU x 8 GB |
| 2 vCPU x 16 GB |
| 3 vCPU x 24 GB |
| 4 vCPU x 32GB|

The default vCPU to memory combinations are:
- Default Spark driver size: 1vCPU and 4GB memory
- Default Spark executor size: 1vCPU and 4GB memory
