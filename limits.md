---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-23"

subcollection: AnalyticsEngine

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}


# Default limits and quotas for {{site.data.keyword.iae_short}} instances
{: #limits}

The following sections provide details about the default limit and quota settings for {{site.data.keyword.iae_full_notm}} serverless instances.
{: shortdesc}

These default values are set to avoid excessive billing, however the values can be set to larger defaults based on user requirements by opening a support ticket.

## Application limits
{: #limits_application}

The following table lists the default limits and quotas for {{site.data.keyword.iae_short}} instances.


| Category                                |        Default         |
| --------------------------------------- | ---------------------- |
| Maximum number of instances per account |                      5 |
| Maximum cores per instance              |                    150 |
| Maximum memory per instances            |                 600 GB |
| Shuffle space per core                  | approx. 30 GB (Not customizable) |
| Maximum time applications can run       | 72 hours (Not customizable) | 
{: caption="Default limits and quotas for {{site.data.keyword.iae_short}} instances" caption-side="top"}



## Supported Spark driver and executor vCPU and memory combinations
{: #cpu-mem-combination}

The {{site.data.keyword.iae_full_notm}} Standard serverless plan for Apache Spark  supports only the following pre-defined Spark driver and executor vCPU and memory combinations.

These two vCPU to memory proportions are supported: 1 vCPU to 4 GB of memory and 1 vCPU to 8 GB of memory.

The following table shows the supported vCPU to memory size combinations.

| 1 : 2 ratio | 1 : 4 ratio | 1 : 8 ratio |
| ------------|-------------|-------------|
| 1 vCPU x 2 GB | 1 vCPU x 4 GB | 1 vCPU x 8 GB |
| 2 vCPU x 4 GB | 2 vCPU x 8 GB | 2 vCPU x 16 GB |
| 3 vCPU x 6 GB | 3 vCPU x 12 GB | 3 vCPU x 24 GB |
| 4 vCPU x 8 GB | 4 vCPU x 16 GB | 4 vCPU x 32 GB |
| 5 vCPU x 10 GB | 5 vCPU x 20 GB | |
| 6 vCPU x 12 GB | 6 vCPU x 24 GB | 6 vCPU x 48 GB|
| 7 vCPU x 14 GB | 7 vCPU x 28 GB | |
| 8 vCPU x 16 GB | 8 vCPU x 32 GB | |
| 10 vCPU x 20 GB | 10 vCPU x 40 GB | |
| 12 vCPU x 24 GB | 12 vCPU x 48 GB | |
{: caption="Supported vCPU to memory size combinations" caption-side="top"}


The default vCPU to memory combinations are:
- Default Spark driver size: 1vCPU and 4GB memory
- Default Spark executor size: 1vCPU and 4GB memory
