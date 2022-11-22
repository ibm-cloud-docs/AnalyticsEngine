---

copyright:
  years: 2019, 2022
lastupdated: "2022-11-21"

keywords: IBM Analytics Engine release notes

subcollection: AnalyticsEngine

content-type: release-note

---
<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}

<!-- keywords values above are place holders. Actual values should be pulled from the release notes entries. -->

{{site.data.keyword.attribute-definition-list}}

<!-- You must add the release-note content type in your attribute definitions AND to each release note H2. This will ensure that the release note entry is pulled into the notifications library. -->

# Release notes for {{site.data.keyword.iae_full_notm}} classic instances
{: #iae-classic-relnotes}

<!-- The title of your H1 should be Release notes for _service-name_, where _service-name_ is the non-trademarked short version conref. Include your service name as a search keyword at the top of your Markdown file. See the example keywords above. -->

Use these release notes to learn about the latest updates to {{site.data.keyword.iae_full_notm}} classic instances that are grouped by date.
{: shortdesc}

## 18 November 2022
{: #AnalyticsEngine-nov1822}
{: release-note}

New security patch for **AE-1.2.v29.26**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.26**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.26**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-37434](https://nvd.nist.gov/vuln/detail/CVE-2022-37434)
    - [CVE-2022-2509](https://nvd.nist.gov/vuln/detail/CVE-2022-2509)
    - [CVE-2022-3515](https://nvd.nist.gov/vuln/detail/CVE-2022-3515)
    - [CVE-2022-35525](https://nvd.nist.gov/vuln/detail/CVE-2022-35525)
    - [CVE-2022-35527](https://nvd.nist.gov/vuln/detail/CVE-2022-35527)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

## October 2022
{: #AnalyticsEngine-oct22}

### 27 October 2022
{: #AnalyticsEngine-oct2722}
{: release-note}

New security patch for **AE-1.2.v29.25**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.25**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.25**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-40674](https://nvd.nist.gov/vuln/detail/CVE-2022-40674)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

### 05 October 2022
{: #AnalyticsEngine-oct0522}
{: release-note}

New security patch for **AE-1.2.v29.24**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.24**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.24**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-2526](https://nvd.nist.gov/vuln/detail/CVE-2022-2526)
    - [CVE-2022-32206](https://nvd.nist.gov/vuln/detail/CVE-2022-32206)
    - [CVE-2022-32208](https://nvd.nist.gov/vuln/detail/CVE-2022-32208)
    - [CVE-2015-20107](https://nvd.nist.gov/vuln/detail/CVE-2015-20107)
    - [CVE-2022-0391](https://nvd.nist.gov/vuln/detail/CVE-2022-0391)
    - [CVE-2022-34903](https://nvd.nist.gov/vuln/detail/CVE-2022-34903)
    - [CVE-2015-20107](https://nvd.nist.gov/vuln/detail/CVE-2015-20107)
    - [CVE-2022-0391](https://nvd.nist.gov/vuln/detail/CVE-2022-0391)
    - [CVE-2022-2526](https://nvd.nist.gov/vuln/detail/CVE-2022-2526)
    - [CVE-2022-32206](https://nvd.nist.gov/vuln/detail/CVE-2022-32206)
    - [CVE-2022-32208](https://nvd.nist.gov/vuln/detail/CVE-2022-32208)
    - [CVE-2022-34903](https://nvd.nist.gov/vuln/detail/CVE-2022-34903)
          
    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

## 02 September 2022
{: #AnalyticsEngine-sep0222}
{: release-note}

New security patch for **AE-1.2.v29.23**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.23**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.23**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-21540](https://nvd.nist.gov/vuln/detail/CVE-2022-21540)
    - [CVE-2022-21541](https://nvd.nist.gov/vuln/detail/CVE-2022-21541)
    - [CVE-2022-34169](https://nvd.nist.gov/vuln/detail/CVE-2022-34169)
    - [CVE-2022-37434](https://nvd.nist.gov/vuln/detail/CVE-2022-37434)
    - [CVE-2022-1586](https://nvd.nist.gov/vuln/detail/CVE-2022-1586)
    - [CVE-2022-1292](https://nvd.nist.gov/vuln/detail/CVE-2022-1292)
    - [CVE-2022-2068](https://nvd.nist.gov/vuln/detail/CVE-2022-2068)
    - [CVE-2022-2097](https://nvd.nist.gov/vuln/detail/CVE-2022-2097)
    - [CVE-2022-1785](https://nvd.nist.gov/vuln/detail/CVE-2022-1785)
    - [CVE-2022-1897](https://nvd.nist.gov/vuln/detail/CVE-2022-1897)
    - [CVE-2022-1927](https://nvd.nist.gov/vuln/detail/CVE-2022-1927) 
          
    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.


## August 2022
{: #AnalyticsEngine-aug22}

### 25 August 2022
{: #AnalyticsEngine-aug2522}
{: release-note}

Shutdown of the London and Sydney regions for the IBM Analytics Engine standard-hourly and standard-monthly plans
:   From September 12th, 2022, you can no longer provision Analytics Engine service standard-hourly and standard-monthly plans on IBM Cloud in the London and Sydney regions.

    Existing users who were added to an allowlist will be able to continue to provision service instances for Analytics Engine standard hourly and standard monthly plans in the regions Dallas, Washington, Frankfurt and Tokyo until the November 9th, 2022. For key dates and other details, see [IBM Analytics Engine announcements](https://cloud.ibm.com/status/announcement?component=ibmanalyticsengine).

    Start using the new serverless plan for a consumption-based usage model where you can optimize resource utilization and control your costs. To get started using the serverless plan, see [Getting started using serverless {{site.data.keyword.iae_full_notm}} instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-getting-started).


### 03 August 2022
{: #AnalyticsEngine-aug0322}
{: release-note}

New security patch for **AE-1.2.v29.22**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.22**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.22**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2009-5155](https://nvd.nist.gov/vuln/detail/CVE-2009-5155)
    - [CVE-2016-5011](https://nvd.nist.gov/vuln/detail/CVE-2016-5011)
    - [CVE-2017-10684](https://nvd.nist.gov/vuln/detail/CVE-2017-10684)
    - [CVE-2017-10685](https://nvd.nist.gov/vuln/detail/CVE-2017-10685)
    - [CVE-2017-11112](https://nvd.nist.gov/vuln/detail/CVE-2017-11112)
    - [CVE-2017-11113](https://nvd.nist.gov/vuln/detail/CVE-2017-11113)
    - [CVE-2017-13728](https://nvd.nist.gov/vuln/detail/CVE-2017-13728)
    - [CVE-2017-13729](https://nvd.nist.gov/vuln/detail/CVE-2017-13729)
    - [CVE-2017-13730](https://nvd.nist.gov/vuln/detail/CVE-2017-13730)
    - [CVE-2017-13731](https://nvd.nist.gov/vuln/detail/CVE-2017-13731)
    - [CVE-2017-13732](https://nvd.nist.gov/vuln/detail/CVE-2017-13732)
    - [CVE-2017-13733](https://nvd.nist.gov/vuln/detail/CVE-2017-13733)
    - [CVE-2017-13734](https://nvd.nist.gov/vuln/detail/CVE-2017-13734)
    - [CVE-2017-16879](https://nvd.nist.gov/vuln/detail/CVE-2017-16879)
    - [CVE-2017-12424](https://nvd.nist.gov/vuln/detail/CVE-2017-12424)
    - [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482)
    - [CVE-2018-25032](https://nvd.nist.gov/vuln/detail/CVE-2018-25032)
    - [CVE-2018-7169](https://nvd.nist.gov/vuln/detail/CVE-2018-7169)
    - [CVE-2018-19211](https://nvd.nist.gov/vuln/detail/CVE-2018-19211)
    - [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923)
    - [CVE-2019-17594](https://nvd.nist.gov/vuln/detail/CVE-2019-17594)
    - [CVE-2019-17595](https://nvd.nist.gov/vuln/detail/CVE-2019-17595)
    - [CVE-2019-18276](https://nvd.nist.gov/vuln/detail/CVE-2019-18276)
    - [CVE-2019-20838](https://nvd.nist.gov/vuln/detail/CVE-2019-20838)
    - [CVE-2020-14155](https://nvd.nist.gov/vuln/detail/CVE-2020-14155)
    - [CVE-2020-27350](https://nvd.nist.gov/vuln/detail/CVE-2020-27350)
    - [CVE-2020-13529](https://nvd.nist.gov/vuln/detail/CVE-2020-13529)
    - [CVE-2020-6096](https://nvd.nist.gov/vuln/detail/CVE-2020-6096)
    - [CVE-2021-40528](https://nvd.nist.gov/vuln/detail/CVE-2021-40528)
    - [CVE-2021-3520](https://nvd.nist.gov/vuln/detail/CVE-2021-3520)
    - [CVE-2021-3999](https://nvd.nist.gov/vuln/detail/CVE-2021-3999)
    - [CVE-2021-39537](https://nvd.nist.gov/vuln/detail/CVE-2021-39537)
    - [CVE-2021-36084](https://nvd.nist.gov/vuln/detail/CVE-2021-36084)
    - [CVE-2021-36085](https://nvd.nist.gov/vuln/detail/CVE-2021-36085)
    - [CVE-2021-36086](https://nvd.nist.gov/vuln/detail/CVE-2021-36086)
    - [CVE-2021-36087](https://nvd.nist.gov/vuln/detail/CVE-2021-36087)
    - [CVE-2021-20193](https://nvd.nist.gov/vuln/detail/CVE-2021-20193)
    - [CVE-2021-33910](https://nvd.nist.gov/vuln/detail/CVE-2021-33910)
    - [CVE-2021-33560](https://nvd.nist.gov/vuln/detail/CVE-2021-33560)
    - [CVE-2022-2097](https://nvd.nist.gov/vuln/detail/CVE-2022-2097)
    - [CVE-2022-23218](https://nvd.nist.gov/vuln/detail/CVE-2022-23218)
    - [CVE-2022-23219](https://nvd.nist.gov/vuln/detail/CVE-2022-23219)
    - [CVE-2022-22576](https://nvd.nist.gov/vuln/detail/CVE-2022-22576)
    - [CVE-2022-27774](https://nvd.nist.gov/vuln/detail/CVE-2022-27774)
    - [CVE-2022-27776](https://nvd.nist.gov/vuln/detail/CVE-2022-27776)
    - [CVE-2022-27782](https://nvd.nist.gov/vuln/detail/CVE-2022-27782)
    - [CVE-2022-29824](https://nvd.nist.gov/vuln/detail/CVE-2022-29824)
    - [CVE-2022-25313](https://nvd.nist.gov/vuln/detail/CVE-2022-25313)
    - [CVE-2022-25314](https://nvd.nist.gov/vuln/detail/CVE-2022-25314)
    - [CVE-2022-1621](https://nvd.nist.gov/vuln/detail/CVE-2022-1621)
    - [CVE-2022-1629](https://nvd.nist.gov/vuln/detail/CVE-2022-1629) 
    - [CVE-2022-29458](https://nvd.nist.gov/vuln/detail/CVE-2022-29458)
    - [CVE-2022-1271](https://nvd.nist.gov/vuln/detail/CVE-2022-1271)
    - [CVE-2022-34903](https://nvd.nist.gov/vuln/detail/CVE-2022-34903)
    - [CVE-2022-1664](https://nvd.nist.gov/vuln/detail/CVE-2022-1664)
    - [CVE-2022-1304](https://nvd.nist.gov/vuln/detail/CVE-2022-1304)
    
          
    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.


## 04 July 2022
{: #AnalyticsEngine-jul0422}
{: release-note}

New security patch for **AE-1.2.v29.21**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.21**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.21**.

    The CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-1271](https://nvd.nist.gov/vuln/detail/CVE-2022-1271)
          
    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.


## 13 May 2022
{: #AnalyticsEngine-may1622}
{: release-note}

New security patch for **AE-1.2.v29.20**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.20**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.20**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2018-25032](https://nvd.nist.gov/vuln/detail/CVE-2018-25032)
    - [CVE-2022-1271](https://nvd.nist.gov/vuln/detail/CVE-2022-1271)
    - [RHSA-2022:1537](https://access.redhat.com/errata/RHSA-2022:1537)
    - [RHSA-2022:1642](https://access.redhat.com/errata/RHSA-2022:1642)
    - [CVE-2019-18276](https://nvd.nist.gov/vuln/detail/CVE-2019-18276)
    - [CVE-2021-36084](https://nvd.nist.gov/vuln/detail/CVE-2021-36084)
    - [CVE-2021-36085](https://nvd.nist.gov/vuln/detail/CVE-2021-36085)
    - [CVE-2021-36086](https://nvd.nist.gov/vuln/detail/CVE-2021-36086)
    - [CVE-2021-36087](https://nvd.nist.gov/vuln/detail/CVE-2021-36087)
    - [CVE-2022-1271](https://nvd.nist.gov/vuln/detail/CVE-2022-1271)
    - [CVE-2022-1292](https://nvd.nist.gov/vuln/detail/CVE-2022-1292)
    - [CVE-2022-1343](https://nvd.nist.gov/vuln/detail/CVE-2022-1343)
    - [CVE-2022-1434](https://nvd.nist.gov/vuln/detail/CVE-2022-1434)
    - [CVE-2022-1473](https://nvd.nist.gov/vuln/detail/CVE-2022-1473)
     
     
    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

## April 2022
{: #AnalyticsEngine-apr22}

### 11 April 2022
{: #AnalyticsEngine-apr1122}
{: release-note}

New security patch for **AE-1.2.v29.19**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.19**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.19**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2022-28391](https://nvd.nist.gov/vuln/detail/CVE-2022-28391)
    - [CVE-2022-0778](https://nvd.nist.gov/vuln/detail/CVE-2022-0778)
    - [CVE-2018-25032](https://nvd.nist.gov/vuln/detail/CVE-2018-25032)
    - [CVE-2021-45960](https://nvd.nist.gov/vuln/detail/CVE-2021-45960)
    - [CVE-2021-46143](https://nvd.nist.gov/vuln/detail/CVE-2021-46143)
    - [CVE-2022-22822](https://nvd.nist.gov/vuln/detail/CVE-2022-22822)
    - [CVE-2022-22823](https://nvd.nist.gov/vuln/detail/CVE-2022-22823)
    - [CVE-2022-22824](https://nvd.nist.gov/vuln/detail/CVE-2022-22824)
    - [CVE-2022-22825](https://nvd.nist.gov/vuln/detail/CVE-2022-22825)
    - [CVE-2022-22826](https://nvd.nist.gov/vuln/detail/CVE-2022-22826)
    - [CVE-2022-22827](https://nvd.nist.gov/vuln/detail/CVE-2022-22827)
    - [CVE-2022-23852](https://nvd.nist.gov/vuln/detail/CVE-2022-23852)
    - [CVE-2022-25235](https://nvd.nist.gov/vuln/detail/CVE-2022-25235)
    - [CVE-2022-25236](https://nvd.nist.gov/vuln/detail/CVE-2022-25236)
    - [CVE-2022-25315](https://nvd.nist.gov/vuln/detail/CVE-2022-25315)
    - [CVE-2022-23308](https://nvd.nist.gov/vuln/detail/CVE-2022-23308)
    - [CVE-2021-3999](https://nvd.nist.gov/vuln/detail/CVE-2021-3999)
    - [CVE-2022-23218](https://nvd.nist.gov/vuln/detail/CVE-2022-22218)
    - [CVE-2022-23219](https://nvd.nist.gov/vuln/detail/CVE-2022-23219)
    - [CVE-2021-23177](https://nvd.nist.gov/vuln/detail/CVE-2021-23177)
    - [CVE-2021-31566](https://nvd.nist.gov/vuln/detail/CVE-2021-31566)
    - [CVE-2022-0778](https://nvd.nist.gov/vuln/detail/CVE-2022-0778)
    - [CVE-2021-20193](https://nvd.nist.gov/vuln/detail/CVE-2021-20193)


    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

### 07 April 2022 
{: #AnalyticsEngine-apr0722}
{: release-note}

New users can no longer create IBM Analytics Engine service instances using the classic plans
:   The IBM Analytics Engine Classic plans are deprecated and will be removed later this year. 

    New users will not be able to create Lite, Standard-Hourly, or Standard-Monthly IBM Analytics Engine instances beginning 7 April 2022. As a new user, create serverless instances, by selecting the Standard Serverless for Apache Spark plan, to run your Spark applications.

    Existing users can still continue creating IBM Analytics Engine service instances using the classic plans.


Migrating from classic instances
:   To help you move from using classic instances to using serverless instances, see [Differences between classic and serverless instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-differences-between-instances) and the topics under **Migrating from classic instances**.

## 10 March 2022
{: #AnalyticsEngine-mar1022}
{: release-note}

New security patch for **AE-1.2.v29.18**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.18**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.18**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-44142](https://access.redhat.com/security/cve/CVE-2021-44142)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

## February 2022
{: #AnalyticsEngine-feb22}

### 17 February 2022
{: #AnalyticsEngine-feb1722}
{: release-note}

New monitoring system for **AE-1.2.v29.17**
:   All clusters that were created using standard-hourly and standard-monthly plans, with the version **AE-1.2.v29.17** or later, now use a new monitoring system, which is required by IBM Cloud. Proactive monitoring of older clusters, created before 11 February 2022, 15:00 UTC, will be discontinued starting 25 February 2022, 11:30 UTC. 
 
    You can still get support for older clusters through IBM Cloud support tickets. However, as proactive monitoring of these older cluster will be unavailable, you should delete all clusters that were created before 11 February 2022, 15:00 UTC and create new ones to benefit from seamless proactive monitoring available in all new IBM Cloud Analytics Engine clusters.

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

### 07 February 2022
{: #AnalyticsEngine-feb0722}
{: release-note}

New security patch for **AE-1.2.v29.16**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.16**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.16**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-3733](https://access.redhat.com/security/cve/CVE-2021-3733)
    - [CVE-2021-3737](https://access.redhat.com/security/cve/CVE-2021-3737)

    The Python version on clusters was upgraded to Python 3.7.11. All of the dependent packages were upgraded as well.

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

## December 2021
{: #AnalyticsEngine-dec21}

### 16 December 2021
{: #AnalyticsEngine-dec1621}
{: release-note}

New security patch for **AE-1.2.v29.15**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.15**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.15**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-44228](https://access.redhat.com/security/cve/CVE-2021-44228)

        Based on the advice received from the underlying engine, Hortonworks Data Platform(HDP), the instructions for removing the vulnerable class from log4j 2.x jars for Oozie and Hive were applied. Note that log4j 1.x is not affected by `CVE-2021-44228`. 

        The changes are available in all new clusters built after 16 December 2021. To avail the fix, discard all old clusters and create new ones.

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) to recycle your clusters regularly.

### 08 December 2021
{: #AnalyticsEngine-dec0821}
{: release-note}

New security patches for **AE-1.2.v29.14**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.14**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.14**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-42574](https://nvd.nist.gov/vuln/detail/CVE-2021-42574)
    - [RHSA-2021:4587](https://access.redhat.com/errata/RHSA-2021:4587)
    - [CVE-2021-28831](https://nvd.nist.gov/vuln/detail/CVE-2021-28831)
    - [CVE-2021-42374](https://nvd.nist.gov/vuln/detail/CVE-2021-42374)
    - [CVE-2021-42375](https://nvd.nist.gov/vuln/detail/CVE-2021-42375)
    - [CVE-2021-42378](https://nvd.nist.gov/vuln/detail/CVE-2021-42378)
    - [CVE-2021-42379](https://nvd.nist.gov/vuln/detail/CVE-2021-42379)
    - [CVE-2021-42380](https://nvd.nist.gov/vuln/detail/CVE-2021-42380)
    - [CVE-2021-42381](https://nvd.nist.gov/vuln/detail/CVE-2021-42381)
    - [CVE-2021-42382](https://nvd.nist.gov/vuln/detail/CVE-2021-42382)
    - [CVE-2021-42383](https://nvd.nist.gov/vuln/detail/CVE-2021-42383)
    - [CVE-2021-42384](https://nvd.nist.gov/vuln/detail/CVE-2021-42384)
    - [CVE-2021-42385](https://nvd.nist.gov/vuln/detail/CVE-2021-42385)
    - [CVE-2021-42386](https://nvd.nist.gov/vuln/detail/CVE-2021-42386)
    - [CVE-2016-4658](https://nvd.nist.gov/vuln/detail/CVE-2016-4658)
    - [CVE-2021-42574](https://nvd.nist.gov/vuln/detail/CVE-2021-42574)
    - [CVE-2021-35550](https://nvd.nist.gov/vuln/detail/CVE-2021-35550)
    - [CVE-2021-35556](https://nvd.nist.gov/vuln/detail/CVE-2021-35556)
    - [CVE-2021-35559](https://nvd.nist.gov/vuln/detail/CVE-2021-35559)
    - [CVE-2021-35561](https://nvd.nist.gov/vuln/detail/CVE-2021-35561)
    - [CVE-2021-35564](https://nvd.nist.gov/vuln/detail/CVE-2021-35564)
    - [CVE-2021-35565](https://nvd.nist.gov/vuln/detail/CVE-2021-35565)
    - [CVE-2021-35567](https://nvd.nist.gov/vuln/detail/CVE-2021-35567)
    - [CVE-2021-35578](https://nvd.nist.gov/vuln/detail/CVE-2021-35578)
    - [CVE-2021-35586](https://nvd.nist.gov/vuln/detail/CVE-2021-35586)
    - [CVE-2021-35588](https://nvd.nist.gov/vuln/detail/CVE-2021-35588)
    - [CVE-2021-35603](https://nvd.nist.gov/vuln/detail/CVE-2021-35603)
    - [CVE-2021-22543](https://nvd.nist.gov/vuln/detail/CVE-2021-22543)
    - [CVE-2021-3653](https://nvd.nist.gov/vuln/detail/CVE-2021-3653)
    - [CVE-2021-3656](https://nvd.nist.gov/vuln/detail/CVE-2021-3656)
    - [CVE-2021-37576](https://nvd.nist.gov/vuln/detail/CVE-2021-37576)
    - [CVE-2020-36385](https://nvd.nist.gov/vuln/detail/CVE-2020-36385)
    - [CVE-2021-20271](https://nvd.nist.gov/vuln/detail/CVE-2021-20271)
    - [CVE-2021-37750](https://nvd.nist.gov/vuln/detail/CVE-2021-37750)
    - [CVE-2021-41617](https://nvd.nist.gov/vuln/detail/CVE-2021-41617)
    - [RHSA-2021:4777](https://access.redhat.com/errata/RHSA-2021:4777)
    - [RHSA-2021:4782](https://access.redhat.com/errata/RHSA-2021:4782)
    - [RHSA-2021:4785](https://access.redhat.com/errata/RHSA-2021:4785)
    - [RHSA-2021:4788](https://access.redhat.com/errata/RHSA-2021:4788)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.


## 15 November 2021
{: #AnalyticsEngine-nov1521}
{: release-note}

New security patches for **AE-1.2.v29.13**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.13**, your cluster is vulnerable to CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.13**, which includes general security fixes.

Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## October 2021
{: #AnalyticsEngine-oct21}

### 29 October 2021
{: #AnalyticsEngine-oct2921}
{: release-note}

New security patches for **AE-1.2.v29.12**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.12**, your cluster is vulnerable to CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.12**, which includes general security fixes.

Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.


### 1 October 2021
{: #AnalyticsEngine-oct0121}
{: release-note}

New security patches for **AE-1.2.v29.11**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.11**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.11**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-36222](https://nvd.nist.gov/vuln/detail/CVE-2021-36222)
    - [CVE-2021-37750](https://nvd.nist.gov/vuln/detail/CVE-2021-37750)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 17 September 2021
{: #AnalyticsEngine-sep1721}
{: release-note}

New security patches for **AE-1.2.v29.10**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.10**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.10**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-25214](https://nvd.nist.gov/vuln/detail/CVE-2021-25214)
    - [CVE-2021-3621](https://nvd.nist.gov/vuln/detail/CVE-2021-3621)
    - [RHSA-2021:3325](https://access.redhat.com/errata/RHSA-2021:3325)
    - [RHSA-2021:3336](https://access.redhat.com/errata/RHSA-2021:3336)
    - [CVE-2020-13529](https://nvd.nist.gov/vuln/detail/CVE-2020-13529)
    - [CVE-2021-33560](https://nvd.nist.gov/vuln/detail/CVE-2021-33560)
    - [CVE-2021-33910](https://nvd.nist.gov/vuln/detail/CVE-2021-33910)
    - [CVE-2021-40528](https://nvd.nist.gov/vuln/detail/CVE-2021-40528)
    - [CVE-2021-20193](https://nvd.nist.gov/vuln/detail/CVE-2021-20193)
    - [CVE-2017-12424](https://nvd.nist.gov/vuln/detail/CVE-2017-12424)
    - [CVE-2018-7169](https://nvd.nist.gov/vuln/detail/CVE-2018-7169)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 26 August 2021
{: #AnalyticsEngine-aug2621}
{: release-note}

New security patches for **AE-1.2.v29.9**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.9**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.9**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-36159](https://nvd.nist.gov/vuln/detail/CVE-2021-36159)
    - [CVE-2021-3711](https://nvd.nist.gov/vuln/detail/CVE-2021-3711)
    - [CVE-2021-3712](https://nvd.nist.gov/vuln/detail/CVE-2021-3712)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 08 July 2021
{: #AnalyticsEngine-jul0821}
{: release-note}

New security patches for **AE-1.2.v29.8**
:   **Important**:  Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.8**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.8**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-3520](https://nvd.nist.gov/vuln/detail/CVE-2021-3520)
    - [CVE-2021-20254](https://nvd.nist.gov/vuln/detail/CVE-2021-20254)
    - [CVE-2021-27219](https://nvd.nist.gov/vuln/detail/CVE-2021-27219)
    - [CVE-2020-12362](https://nvd.nist.gov/vuln/detail/CVE-2020-12362)
    - [CVE-2020-12363](https://nvd.nist.gov/vuln/detail/CVE-2020-12363)
    - [CVE-2020-12364](https://nvd.nist.gov/vuln/detail/CVE-2020-12364)
    - [CVE-2020-27170](https://nvd.nist.gov/vuln/detail/CVE-2020-27170)
    - [CVE-2020-8648](https://nvd.nist.gov/vuln/detail/CVE-2020-8648)
    - [CVE-2021-3347](https://nvd.nist.gov/vuln/detail/CVE-2021-3347)
    - [CVE-2019-10208](https://nvd.nist.gov/vuln/detail/CVE-2019-10208)
    - [CVE-2020-25694](https://nvd.nist.gov/vuln/detail/CVE-2020-25694)
    - [CVE-2020-25695](https://nvd.nist.gov/vuln/detail/CVE-2020-25695)        
    - [RHSA-2021:2147](https://access.redhat.com/errata/RHSA-2021:2147)
    - [RHSA-2021:2313](https://access.redhat.com/errata/RHSA-2021:2313)
    - [RHSA-2021:2314](https://access.redhat.com/errata/RHSA-2021:2314)
    - [RHSA-2021:1512](https://access.redhat.com/errata/RHSA-2021:1512)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 21 May 2021
{: #AnalyticsEngine-may2121}
{: release-note}

New security patches for **AE-1.2.v29.7**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.7**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.7**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2009-5155](https://nvd.nist.gov/vuln/detail/CVE-2009-5155)
    - [CVE-2020-6096](https://nvd.nist.gov/vuln/detail/CVE-2020-6096)
    - [CVE-2019-11719](https://nvd.nist.gov/vuln/detail/CVE-2019-11719)
    - [CVE-2019-11727](https://nvd.nist.gov/vuln/detail/CVE-2019-11727)
    - [CVE-2019-11756](https://nvd.nist.gov/vuln/detail/CVE-2019-11756)
    - [CVE-2019-17006](https://nvd.nist.gov/vuln/detail/CVE-2019-17006)
    - [CVE-2019-17023](https://nvd.nist.gov/vuln/detail/CVE-2019-17023)
    - [CVE-2020-12400](https://nvd.nist.gov/vuln/detail/CVE-2020-12400)
    - [CVE-2020-12401](https://nvd.nist.gov/vuln/detail/CVE-2020-12401)
    - [CVE-2020-12402](https://nvd.nist.gov/vuln/detail/CVE-2020-12402)
    - [CVE-2020-12403](https://nvd.nist.gov/vuln/detail/CVE-2020-12403)
    - [CVE-2020-6829](https://nvd.nist.gov/vuln/detail/CVE-2020-6829)
    - [CVE-2021-25215](https://nvd.nist.gov/vuln/detail/CVE-2020-25215)
    - [RHSA-2020:4076](https://access.redhat.com/errata/RHSA-2020:4076)
    - [RHSA-2021:1469](https://access.redhat.com/errata/RHSA-2021:1469)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## April 2021
{: #AnalyticsEngine-apr21}

### 28 April 2021
{: #AnalyticsEngine-apr2821}
{: release-note}

New security patches for **AE-1.2.v29.6**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.6**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.6**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-30139](https://nvd.nist.gov/vuln/detail/CVE-2021-30139)
    - [CVE-2021-20277](https://nvd.nist.gov/vuln/detail/CVE-2021-20277)
    - [CVE-2021-27363](https://nvd.nist.gov/vuln/detail/CVE-2021-27363)
    - [CVE-2021-27364](https://nvd.nist.gov/vuln/detail/CVE-2021-27364)
    - [CVE-2021-27365](https://nvd.nist.gov/vuln/detail/CVE-2021-27365)
    - [CVE-2021-20305](https://nvd.nist.gov/vuln/detail/CVE-2021-20305)
    - [CVE-2021-2163](https://nvd.nist.gov/vuln/detail/CVE-2021-2163)
    - [RHSA-2021:1071](https://access.redhat.com/errata/RHSA-2021:1071)
    - [RHSA-2021:1072](https://access.redhat.com/errata/RHSA-2021:1072)
    - [RHSA-2021:1145](https://access.redhat.com/errata/RHSA-2021:1145)
    - [RHSA-2021:1298](https://access.redhat.com/errata/RHSA-2021:1298)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

### 15 April 2021
{: #AnalyticsEngine-apr1521}
{: release-note}

New security patches for **AE-1.2.v29.5**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.5**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.5**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2021-3449](https://nvd.nist.gov/vuln/detail/CVE-2021-3449)
    - [CVE-2021-3450](https://nvd.nist.gov/vuln/detail/CVE-2021-3450)
    - [CVE-2019-19532](https://nvd.nist.gov/vuln/detail/CVE-2019-19532)
    - [CVE-2020-0427](https://nvd.nist.gov/vuln/detail/CVE-2020-0427)
    - [CVE-2020-14351](https://nvd.nist.gov/vuln/detail/CVE-2020-14351)
    - [CVE-2020-25211](https://nvd.nist.gov/vuln/detail/CVE-2020-25211)
    - [CVE-2020-25645](https://nvd.nist.gov/vuln/detail/CVE-2020-25645)
    - [CVE-2020-25656](https://nvd.nist.gov/vuln/detail/CVE-2020-25656)
    - [CVE-2020-25705](https://nvd.nist.gov/vuln/detail/CVE-2020-25705)
    - [CVE-2020-28374](https://nvd.nist.gov/vuln/detail/CVE-2020-28374)
    - [CVE-2020-29661](https://nvd.nist.gov/vuln/detail/CVE-2020-29661)
    - [CVE-2020-7053](https://nvd.nist.gov/vuln/detail/CVE-2020-7053)
    - [CVE-2021-20265](https://nvd.nist.gov/vuln/detail/CVE-2021-20265)
    - [RHSA-2021:0856](https://access.redhat.com/errata/RHSA-2021:0856)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

{{site.data.keyword.iae_full_notm}} is now SOC1 Type 2 and SOC2 Type 2 compliant.
:   For details, see [Compliance](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-compliance#compliance).

## March 2021
{: #AnalyticsEngine-mar21}

### 23 March 2021
{: #AnalyticsEngine-mar2321}
{: release-note}

New security patches for **AE-1.2.v29.4**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.4**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.4**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2018-20482](https://nvd.nist.gov/vuln/detail/CVE-2018-20482)
    - [CVE-2019-9923](https://nvd.nist.gov/vuln/detail/CVE-2019-9923)
    - [CVE-2021-3177](https://nvd.nist.gov/vuln/detail/CVE-2021-3177). Note that for this CVE, Python was upgraded from 3.9 to 3.10.
    - [CVE-2020-8625](https://nvd.nist.gov/vuln/detail/CVE-2020-8625)
    - [RHSA-2021:0671](https://access.redhat.com/errata/RHSA-2021:0671)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.   

The data skipping library was open sourced and can now be used when developing applications with Spark SQL.
:   See [Data skipping for Spark SQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-data-skipping).

### 22 March 2021
{: #AnalyticsEngine-mar2221}
{: release-note}

You can now create {{site.data.keyword.iae_short}} clusters with auto scaling that will automatically scale nodes up or down in the cluster based on the amount of memory demanded by the applications.
:   See [Provisioning an auto scaling cluster](/docs/AnalyticsEngine?topic=AnalyticsEngine-autoscaling-clusters).

You can now also scale down a cluster manually by using the cluster resize operation.
:   See [Resizing clusters](/docs/AnalyticsEngine?topic=AnalyticsEngine-resize-clusters).

You can now provision {{site.data.keyword.iae_full}} instances in two new regions.
:   The new {{site.data.keyword.Bluemix_notm}} regions are:
    - `us-east` (Washington DC)
    - `au-syd` (Sydney)

    For a list of all the supported regions, see [Provisioning {{site.data.keyword.iae_full_notm}} service instances](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE).

## February 2021
{: #AnalyticsEngine-feb21}

### 25 February 2021
{: #AnalyticsEngine-feb2521}
{: release-note}

All biased IT terminology in {{site.data.keyword.iae_full_notm}} is deprecated and will be removed in favor of more inclusive language.
:   This includes changes to terms in the {{site.data.keyword.iae_full_notm}}  documentation and to APIs and cluster node names used in the product itself. See the [Announcement letter](https://cloud.ibm.com/status/announcement?component=ibmanalyticsengine) released end of January 2021.

    **What will be changing**:
    - All occurrences of the term `whitelist` in the documentation have been be replaced by `allowlist`.
    - The API `private_endpoint_whitelist` has been replaced by `private_endpoint_allowlist`.
    - The terms `management-slave1` and `management-slave2` for the "mn002" and "mn003" nodes in a cluster have been replaced by `management2` and `management3`.
    - The environment variable `NODE_TYPE` and it's values `management-slave1` and `management-slave2` has been replaced by `NODE_TYP`. The new values for `NODE_TYP` are `management2` and `management3`.
    - The `master management` node has been changed to `management1` to align with the other management node name changes.

    **Note**:
    - The deprecation period ends at the end of March 2021.
    - Although you can still use the deprecated API and node names until the end of March 2021, you should start using the new API and recreate clusters that use the new node names. See [Using an allowlist to control network traffic](/docs/AnalyticsEngine?topic=AnalyticsEngine-allowlist-to-cluster-access) for details on the allowlist API.

You should migrate your customization scripts to use the new environment variable `NODE_TYP`.
:   See [Customizing a cluster](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-cluster).

### 24 February 2021
{: #AnalyticsEngine-feb2421}
{: release-note}

New security patches for **AE-1.2.v29.3**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.3**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.3**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2019-25013](https://nvd.nist.gov/vuln/detail/CVE-2019-25013)
    - [CVE-2020-10029](https://nvd.nist.gov/vuln/detail/CVE-2020-10029)
    - [CVE-2020-10543](https://nvd.nist.gov/vuln/detail/CVE-2020-10543)
    - [CVE-2020-10878](https://nvd.nist.gov/vuln/detail/CVE-2020-10878)
    - [CVE-2020-12723](https://nvd.nist.gov/vuln/detail/CVE-2020-12723)
    - [CVE-2020-29573](https://nvd.nist.gov/vuln/detail/CVE-2020-29573)
    - [RHSA-2021:0343](https://access.redhat.com/errata/RHSA-2021:0343)
    - [RHSA-2021:0348](https://access.redhat.com/errata/RHSA-2021:0348)
    - [CVE-2021-23839](https://nvd.nist.gov/vuln/detail/CVE-2021-23839)
    - [CVE-2021-23840](https://nvd.nist.gov/vuln/detail/CVE-2021-23840)
    - [CVE-2021-23841](https://nvd.nist.gov/vuln/detail/CVE-2021-23841)
    - [CVE-2019-18282](https://nvd.nist.gov/vuln/detail/CVE-2019-18282)
    - [CVE-2020-10769](https://nvd.nist.gov/vuln/detail/CVE-2020-10769)
    - [CVE-2020-14314](https://nvd.nist.gov/vuln/detail/CVE-2020-14314)
    - [CVE-2020-14318](https://nvd.nist.gov/vuln/detail/CVE-2020-14318)
    - [CVE-2020-14323](https://nvd.nist.gov/vuln/detail/CVE-2020-14323)
    - [CVE-2020-14385](https://nvd.nist.gov/vuln/detail/CVE-2020-14385)
    - [CVE-2020-1472](https://nvd.nist.gov/vuln/detail/CVE-2020-1472)  
    - [CVE-2020-1971](https://nvd.nist.gov/vuln/detail/CVE-2020-1971)  
    - [CVE-2020-24394](https://nvd.nist.gov/vuln/detail/CVE-2020-24394)
    - [CVE-2020-25212](https://nvd.nist.gov/vuln/detail/CVE-2020-25212)
    - [CVE-2020-25643](https://nvd.nist.gov/vuln/detail/CVE-2020-25643)
    - [CVE-2021-3156](https://nvd.nist.gov/vuln/detail/CVE-2021-3156)
    - [RHSA-2020:5437](https://access.redhat.com/errata/RHSA-2020:5437)
    - [RHSA-2020:5439](https://access.redhat.com/errata/RHSA-2020:5439)
    - [RHSA-2020:5566](https://access.redhat.com/errata/RHSA-2020:5566)
    - [RHSA-2021:0221](https://access.redhat.com/errata/RHSA-2021:0221)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 12 January 2021
{: #AnalyticsEngine-jan1221}
{: release-note}

New security patches for **AE-1.2.v29.2**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.2**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.2**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2020-25659](https://bugzilla.redhat.com/show_bug.cgi?id=1889988): the python-cryptography package was upgraded to version 3.3
    - [CVE-2020-28928](https://nvd.nist.gov/vuln/detail/CVE-2020-28928)
    - [CVE-2020-27350](http://www.ubuntu.com/usn/usn-4667-1)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

## 07 December 2020
{: #AnalyticsEngine-dec0720}
{: release-note}

New security patches for **AE-1.2.v29.1**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29.1**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29.1**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2019-20811](https://nvd.nist.gov/vuln/detail/CVE-2019-20811)
    - [CVE-2019-20907](https://nvd.nist.gov/vuln/detail/CVE-2019-20907)
    - [CVE-2020-14331](https://nvd.nist.gov/vuln/detail/CVE-2020-14331)
    - [CVE-2020-17507](https://nvd.nist.gov/vuln/detail/CVE-2020-17507)
    - [CVE-2020-17507](https://nvd.nist.gov/vuln/detail/CVE-2020-17507)
    - [CVE-2020-1935](https://nvd.nist.gov/vuln/detail/CVE-2020-1935)
    - [CVE-2020-8177](https://nvd.nist.gov/vuln/detail/CVE-2020-8177)
    - [CVE-2020-8622](https://nvd.nist.gov/vuln/detail/CVE-2020-8622)
    - [CVE-2020-8623](https://nvd.nist.gov/vuln/detail/CVE-2020-8623)
    - [CVE-2020-8624](https://nvd.nist.gov/vuln/detail/CVE-2020-8624)
    - [RHSA-2020:5002](https://access.redhat.com/errata/RHSA-2020:5002)
    - [RHSA-2020:5009](https://access.redhat.com/errata/RHSA-2020:5009)
    - [RHSA-2020:5011](https://access.redhat.com/errata/RHSA-2020:5011)
    - [CVE-2020-5020](https://nvd.nist.gov/vuln/detail/CVE-2020-5020)
    - [CVE-2020-5021](https://nvd.nist.gov/vuln/detail/CVE-2020-5021)
    - [CVE-2020-5023](https://nvd.nist.gov/vuln/detail/CVE-2020-5023)

    Follow the recommended [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices) and recycle your clusters regularly.

A fix was added that makes the Hive Metastore transparently recover from PostgreSQL metastore database restarts or network interruption issues.
:   If you are externalizing the Hive metastore to IBM Cloud Databases for PostgreSQL, you need to set the socketTimeout value in the JDBC URL. See [Externalizing the Hive metastore to Databases for PostgreSQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-hive-metastore).

Another fix was added so that you no longer need to explicitly grant permissions for the PostgreSQL certificate that you place in `/home/common/wce/clsadmin/` on `mn002`.
:   The folder now has the required permissions and the permissions are retained through restarts.

## October 2020
{: #AnalyticsEngine-oct20}

### 17 October 2020
{: #AnalyticsEngine-oct1720}
{: release-note}

New security patches for **AE-1.2.v29**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v29**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v29**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2020-26116](https://exchange.xforce.ibmcloud.com/vulnerabilities/189404)
    - [CVE-2019-20907](https://exchange.xforce.ibmcloud.com/vulnerabilities/185442)
    - [CVE-2020-14422](https://exchange.xforce.ibmcloud.com/vulnerabilities/184320)

    To apply these fixes, the cluster Python runtime version was upgraded from Python3.7.1-Anaconda3 (conda 4.5.12) distribution to Python3.7.9-Miniconda3.7 (conda 4.8.5) distribution. Note that this implies that the provided Python packages that were present in the earlier Anaconda version will no longer be available out of the box in newly created clusters. If you need to use any of the old packages you need to install the packages explicitly by using the `pip install` command. Remember to follow the recommendations for creating and deleting clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

### 01 October 2020
{: #AnalyticsEngine-oct0120}
{: release-note}

A fix was added to alleviate cluster instability issues caused by an error in an underlying Docker runtime.
:   If you created a cluster between 23 July 2020 and 01 October 2020, you might have experienced intermittent instability that manifested as connectivity issues or down times in nodes. With today's deployment, the runtime version has been replaced by an earlier stable runtime version.

    We strongly urge you to create new clusters and discard all clusters created between 23 July 2020 and 01 October 2020. If for any reason, you need to retain an old cluster, contact support and request for an in-place fix. Note that this in-place fix will require a disruptive restart of the cluster.

    In general, you should following the recommendations for creating and deleting clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

## September 2020
{: #AnalyticsEngine-sep20}

### 17 September 2020
{: #AnalyticsEngine-sep1720}
{: release-note}

A new stocator version was released for **AE-1.2.v28.5**.
:   This includes fixes for the [Spark Dynamic Partition Insert issue](https://github.com/CODAIT/stocator/issues/256) and [Ceph Storage list objects issue](https://github.com/CODAIT/stocator/issues/252).

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v28.5**.

### 10 September 2020
{: #AnalyticsEngine-sep1020}
{: release-note}

New security patches for **AE-1.2.v28.4**
:   **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patch, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v28.4**, your cluster is vulnerable to the following CVE. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v28.4**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2019-17639](https://www.ibm.com/blogs/psirt/security-bulletin-multiple-vulnerabilities-may-affect-ibm-sdk-java-technology-edition-3/)
    - [CVE-2020-14556](https://nvd.nist.gov/vuln/detail/CVE-2020-14556)
    - [CVE-2020-14577](https://nvd.nist.gov/vuln/detail/CVE-2020-14577)
    - [CVE-2020-14578](https://nvd.nist.gov/vuln/detail/CVE-2020-14578)
    - [CVE-2020-14579](https://nvd.nist.gov/vuln/detail/CVE-2020-14579)
    - [CVE-2020-14583](https://nvd.nist.gov/vuln/detail/CVE-2020-14583)
    - [CVE-2020-14593](https://nvd.nist.gov/vuln/detail/CVE-2020-14593)
    - [CVE-2020-14621](https://nvd.nist.gov/vuln/detail/CVE-2020-14621)
    - [RHSA-2020:2968](https://access.redhat.com/errata/RHSA-2020:2968)

You can now configure log aggregation for the HDFS component.
:   See [Configuring log aggregation](/docs/AnalyticsEngine?topic=AnalyticsEngine-log-aggregation).

A fix was added that prevents HDFS audit logs from filling up disk space.
: This was caused by a misconfiguration of the log4j rotation property that disrupted the way clusters should work.

You can now use the time series library in your Spark applications, which provides a rich time series data model and  imputation functions for transforming, reducing, segmenting, joining, and forecasting time series.
:   SQL extensions to time series are also provided. See [Time series library](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series).

## 20 August 2020
{: #AnalyticsEngine-aug20}
{: release-note}

Starting with this deployment, Analytics Engine cluster's build version will be available in `/home/common/aeversion.txt` on the nodes of the cluster.
:   You can check this file after you SSH to the cluster. This will help in tracking fixes that were made available against a particular version of Analytics Engine. For example, this deployment version is AE-1.2.v28.3.

New security patches for **AE-1.2.v28.3**
:   The following patches are available for the security vulnerabilities on the OS level packages of cluster nodes as well as for config vulnerabilities at the OS level.

    **Important**: Security patches are not applied to any existing Analytics Engine clusters. To apply the patches, you need to create new clusters. If the version of your cluster is earlier than **AE-1.2.v28.3**, your cluster is vulnerable to the following CVEs. You can check the build version of your cluster in `/home/common/aeversion.txt` on the nodes of the cluster.

    You are strongly advised to immediately delete existing instances and move to the version **AE-1.2.v28.3**.

    The list of CVEs fixed in this release includes but is not limited to the following:

    - [CVE-2019-11729](https://access.redhat.com/errata/RHSA-2019:4190)
    - [CVE-2019-11745](https://access.redhat.com/errata/RHSA-2019:4190)
    - [CVE-2020-8616](https://access.redhat.com/errata/RHSA-2020:2344)
    - [CVE-2020-8617](https://access.redhat.com/errata/RHSA-2020:2344)
    - [CVE-2019-12735](https://access.redhat.com/errata/RHSA-2019:1619)
    - [CVE-2020-11008](https://access.redhat.com/errata/RHSA-2020:2337)
    - [CVE-2020-12049](https://access.redhat.com/errata/RHSA-2020:2894)
    - [CVE-2020-1967](https://gitlab.alpinelinux.org/alpine/aports/-/issues/11429)
    - [CVE-2020-3810](https://ubuntu.com/security/notices/USN-4359-1)
    - [CVE-2019-5188](http://www.ubuntu.com/usn/usn-4249-1)
    - [CVE-2019-5094](http://www.ubuntu.com/usn/usn-4142-1)
    - [usn-4038-3](http://www.ubuntu.com/usn/usn-4038-3)
    - [CVE-2017-12133](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2017-18269](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2018-11236](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2018-11237](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2018-19591](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2018-6485](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2019-19126](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2019-9169](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2020-10029](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2020-1751](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2020-1752](http://www.ubuntu.com/usn/usn-4416-1)
    - [CVE-2019-13627](http://www.ubuntu.com/usn/usn-4236-2)
    - [CVE-2018-16888](http://www.ubuntu.com/usn/usn-4269-1)
    - [CVE-2019-20386](http://www.ubuntu.com/usn/usn-4269-1)
    - [CVE-2019-3843](http://www.ubuntu.com/usn/usn-4269-1)
    - [CVE-2019-3844](http://www.ubuntu.com/usn/usn-4269-1)
    - [CVE-2020-1712](http://www.ubuntu.com/usn/usn-4269-1)
    - [CVE-2016-9840](http://www.ubuntu.com/usn/usn-4246-1)
    - [CVE-2016-9841](http://www.ubuntu.com/usn/usn-4246-1)
    - [CVE-2016-9842](http://www.ubuntu.com/usn/usn-4246-1)
    - [CVE-2016-9843](http://www.ubuntu.com/usn/usn-4246-1)
    - [CVE-2019-9924](http://www.ubuntu.com/usn/usn-4058-1)
    - [RHSA-2019:4190](https://access.redhat.com/errata/RHSA-2019:4190)
    - [RHSA-2020:2337](https://access.redhat.com/errata/RHSA-2020:2337)
    - [RHSA-2020:2344](https://access.redhat.com/errata/RHSA-2020:2344)
    - [RHSA-2020:2894](https://access.redhat.com/errata/RHSA-2020:2894)

## 23 July 2020
{: #AnalyticsEngine-jul2320}
{: release-note}

Default values for the `gateway.socket.*` parameters were added.
:   This help fix errors related to the length of messages when using Spark WebSockets.

Docker on the underlying Host VMs was upgraded.
: This was done as part of the security fixes that were applied for [CVE-2020-13401](https://exchange.xforce.ibmcloud.com/vulnerabilities/182750).

## May 2020
{: #AnalyticsEngine-may20}

### 22 May 2020
{: #AnalyticsEngine-may2220}
{: release-note}

You can now use allowlist access to a private endpoint cluster.
:   See [Using an allowlist to control network traffic](/docs/AnalyticsEngine?topic=AnalyticsEngine-allowlist-to-cluster-access).

You can now use the {{site.data.keyword.iae_full_notm}} Java SDK to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.
:   See [Using Java](/docs/AnalyticsEngine?topic=AnalyticsEngine-java).

### 17 May 2020
{: #AnalyticsEngine-may1720}
{: release-note}

New security patches
:   Security fixes to the Spark CLI were applied.

### 12 May 2020
{: #AnalyticsEngine-may1220}
{: release-note}

HDP stack for `AE 1.2` was changed
:   The underlying HDP stack for `AE 1.2` was changed from 3.1.0.0-78 to 3.1.5.0-152.

Update if using same IBM Cloud Databases for PostgreSQL instance with all of your {{site.data.keyword.iae_full_notm}} clusters
:   If you associate the same IBM Cloud Databases for PostgreSQL instance with all of your {{site.data.keyword.iae_full_notm}} clusters and you create new clusters after 12 May 2020, you must run the following command on the `mn002` node of the clusters to continue working with the dabasebase instance:

    ```
    /usr/hdp/current/hive-server2/bin/schematool -url 'jdbc:postgresql://<YOUR-POSTGRES-INSTANCE-HOSTNAME>:<PORT>/ibmclouddb?sslmode=verify-ca&sslrootcert=<PATH/TO/POSTGRES/CERT>' -dbType postgres -userName <USERNAME> -passWord <PASSWORD> -upgradeSchema 3.1.1000 -verbose
    ```

    The reason is a database schema version change. You do not have to issue this command if you associate a new IBM Cloud Databases for PostgreSQL instance with the {{site.data.
    keyword.iae_full_notm}} clusters.

Spark and Hive share the same metadata store    
:   Starting from this release, Spark and Hive share the same metadata store.

Security updates to JDK and WLP were applied on all host virtual machines.
:   Also security updates to JDK on the cluster were installed.

Resizing clusters by using the UI or the API fails for clusters that were created before 12 May 2020.
:   The reason is an unexpected incompatibility in the Ambari version between HDP 3.1 (used in clusters created before 12 May 2020) and HDP 3.1.5 (used in clusters created after 12 May 2020). The problem does not affect clusters that were created after 12 May 2020. Those clusters can be resized.

### 05 May 2020
{: #AnalyticsEngine-may0520}
{: release-note}

Support for the {{site.data.keyword.iae_full_notm}} Go SDK is now available which you can use to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.
:   See [Using the {{site.data.keyword.iae_full_notm}} Go SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-go).

## April 2020
{: #AnalyticsEngine-apr20}

### 21 April 2020
{: #AnalyticsEngine-apr2120}
{: release-note}

You can now use the {{site.data.keyword.iae_full_notm}} Node.js and Python SDK to interact programmatically with the {{site.data.keyword.iae_full_notm}} service API.
:   See:

    - For Node.js: [Using the {{site.data.keyword.iae_full_notm}} Node.js SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-node-js)
    - For Python: [Using the {{site.data.keyword.iae_full_notm}} Python SDK](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-python-sdk)

### 03 April 2020
{: #AnalyticsEngine-apr0320}
{: release-note}

Memory tuning enhancements were deployed for standard (default) hardware clusters.
:   If you have standard hardware clusters that you created before 03 April 2020, you need to delete those clusters and create new ones to avoid running into out memory issues or having unresponsive clusters that need to be rebooted by the IBM Support team.

### 02 April 2020
{: #AnalyticsEngine-apr0220}
{: release-note}

You can now use data skipping libraries to boost the performance of Spark SQL queries by associating a summary metadata with each data object.
:   This metadata is used during query evaluation to skip over objects which have no relevant data. See [Data skipping for Spark SQL](/docs/AnalyticsEngine?topic=AnalyticsEngine-data-skipping).

## February 2020
{: #AnalyticsEngine-feb20}

### 13 February 2020
{: #AnalyticsEngine-feb1320}
{: release-note}

Java update
:   Java on clusters was updated to Open JDK.

### 12 February 2020
{: #AnalyticsEngine-feb1220}
{: release-note}

You can enable encryption for data in transit for Spark jobs by explicitly configuring the cluster using a combination of Advanced Options and customization.  
:   See [Enabling Spark jobs encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-spark-encryption).

## January 2020
{: #AnalyticsEngine-jan20}

### 16 January 2020
{: #AnalyticsEngine-jan1620}
{: release-note}

New security fixes
:   Security updates to JDK and WLP were applied on all host virtual machines.

### 14 January 2020
{: #AnalyticsEngine-jan20}
{: release-note}

You can now work with the geospatio-temporal library to expand your data science analysis in Python notebooks to include location analytics by gathering, manipulating and displaying imagery, GPS, satellite photography and historical data.
:   The documentation includes  sample code snippets of many useful functions and links to sample notebooks in the IBM Watson Studio Gallery. See [Working with the spatio-temporal library](/docs/AnalyticsEngine?topic=AnalyticsEngine-geospatial-geotemporal-lib).

Updates were made to best practices around choosing the right Databases for PostgreSQL configuration, configuring the cluster for log monitoring and troubleshooting and using private endpoints.
:   See [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

### 16 December 2019
{: #AnalyticsEngine-dec1619}
{: release-note}

The way that you can use the {{site.data.keyword.iae_full_notm}} Lite plan has changed.
:   The Lite plan is now available only to institutions that have signed up with IBM to try out the Lite plan. See [How to use the Lite plan](/docs/AnalyticsEngine?topic=AnalyticsEngine-general-faqs#lite-plan).

You can now use adhoc PostgreSQL customization scripts to configure the cluster to work with PostgreSQL.
:   See [Running an adhoc customization script for configuring Hive with a Postgres external metastore](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-examples#postgres-metastore).

{{site.data.keyword.iae_full_notm}} now supports Parquet modular encryption that allows encrypting sensitive columns when writing Parquet files and decrypting these columns when reading the encrypted files.
:   Besides ensuring privacy, Parquet encryption also protects the integrity of stored data by detecting any tampering with file contents. See [Working with Parquet encryption](/docs/AnalyticsEngine?topic=AnalyticsEngine-parquet-encryption).

## 25 November 2019
{: #AnalyticsEngine-nov2519}
{: release-note}

You can now use the {{site.data.keyword.Bluemix_notm}} service endpoints feature to securely access your {{site.data.keyword.iae_full_notm}} service instances over the {{site.data.keyword.Bluemix_notm}} private network.
:   You can choose to use either public or private endpoints for the {{site.data.keyword.iae_full_notm}} service. The choice needs to be made at the time you provision the instance. See [Cloud service endpoints integration](/docs/AnalyticsEngine?topic=AnalyticsEngine-service-endpoint-integration).

Fixed broken links
:   Fixed several broken links in Spark history server and Yarn applications.

Other fixes
:   Fixed broken Livy sessions API, benign timeline service errors, tuned Knox for timeout errors.

## 07 October 2019
{: #AnalyticsEngine-oct0719}
{: release-note}

IBM Cloud Databases for PostgreSQL is now available for externalizing Hive cluster metadata.
:   To learn how to configure your cluster to store Hive metadata in PostgreSQL, see [Externalizing the Hive metastore](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#externalizing-hive-metastore).

    You should stop using Compose For MySQL as the Hive metastore.

Hive View has been removed from the underlying platform in `AE 1.2`.
:   You can use any other JDBC UI based client such as SQuirrel SQL or Eclipse Data Source Explorer instead.

## 24 September 2019
{: #AnalyticsEngine-sep2419}
{: release-note}

`AE 1.1` (based on Hortonworks Data Platform 2.6.5) is deprecated.
:   You can no longer provision `AE 1.1` clusters. You also cannot resize and add additional nodes to an `AE 1.1` cluster.

    Although existing clusters will still continue to work and be supported until December 31, 2019,  you should stop using those now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

    All provisioned instances of AE 1.1 will be deleted after December 31, 2019. See the [deprecation notice](https://www.ibm.com/cloud/blog/announcements/deprecation-of-ibm-analytics-engine-v1-1){: external}.

## 21 August 2019
{: #AnalyticsEngine-aug2119}
{: release-note}

To enhance log analysis, log monitoring, and troubleshooting, {{site.data.keyword.iae_full_notm}} now supports aggregating cluster logs and job logs to a centralized {{site.data.keyword.la_short}} server of your choice.
:   See details in [Configuring log aggregation](/docs/AnalyticsEngine?topic=AnalyticsEngine-log-aggregation).

    Note that log aggregation can only be configured for  {{site.data.keyword.iae_full_notm}} clusters created on or after  August 21, 2019.

## 15 May 2019
{: #AnalyticsEngine-may1519}
{: release-note}

A new version of {{site.data.keyword.iae_full_notm}} is now available.
:   `AE  1.2` based on HDP 3.1. It has 3 software packages:

    - `AE 1.2 Hive LLAP`
    - `AE 1.2 Spark and Hive`
    - `AE 1.2 Spark and Hadoop`

    See [Provisioning {{site.data.keyword.iae_full_notm}}](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE).

`AE 1.0` (based on HDP 2.6.2) is deprecated.
:   You can no longer provision `AE 1.0` clusters. You also cannot resize and add additional nodes to an `AE 1.0` cluster.

    Although existing clusters will still continue to work and be supported until September 30, 2019, you should stop using those now and start creating new `AE 1.2` clusters as described in [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

    All provisioned instances of AE 1.0 will be deleted after September 30, 2019.

A new software package `AE 1.2 Hive LLAP` was added to `AE 1.2`, which supports realtime interactive queries.
:   Note however that currently you cannot resize a cluster created using this package.

`AE 1.2` supports Python 3.7.
:   Although `AE 1.1` still supports both Python 3.5 and Python 2.7, you should start moving your Python-based applications or code to Python 3.7 now. Especially considering that the open source community has announced the end of support for Python 2.

`AE 1.2` does not support HDFS encryption zones.
:   To store sensitive data with encryption requirements, select the appropriate COS encryption options. See details in [Best practices](https://cloud.ibm.com/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices#cos-encryption).

## 18 March 2019
{: #AnalyticsEngine-mar1619}
{: release-note}

The cluster credentials can no longer be retrieved via the service endpoints.
:   You can now only retrieve the cluster password by invoking the {{site.data.keyword.iae_full_notm}} [`reset_password`](/docs/AnalyticsEngine?topic=AnalyticsEngine-reset-cluster-password#reset-cluster-password) REST API and you must have the appropriate user permissions.

Changes to environment variable
:   The predefined environment variable `AMBARI_PASSWORD` is no longer available for use in a cluster customization script.

## 06 February 2019
{: #AnalyticsEngine-feb0619}
{: release-note}

The Ambari metrics service (AMS) is now available on all the packages, including the Spark and Hive packages.
:   Previously it was available only on the Hadoop package. This service enables you to see metrics like CPU, memory, and so on, which assists you while troubleshooting the cluster or an application.

Spark dynamic allocation is now enabled by default.
:   The default behavior until now was two executors per Spark application, requiring you to tune cluster resources depending on your application's needs. This behavior has been changed by enabling Spark dynamic allocation, which allows a Spark application to fully utilize the cluster resources. See [Kernel settings](/docs/AnalyticsEngine?topic=AnalyticsEngine-kernel-settings#kernel-settings).

## January 2019
{: #AnalyticsEngine-jan19}

### 30 January 2019
{: #AnalyticsEngine-jan3019}
{: release-note}

{{site.data.keyword.iae_full_notm}} now meets the required IBM controls that are commensurate with the Health Insurance Portability and Accountability Act of 1996 (HIPAA) Security and Privacy Rule requirements.
:   See [HIPAA readiness](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-compliance).

### 24 January 2019
{: #AnalyticsEngine-jan2419}
{: release-note}

Added support for impersonating the user of the Livy APIs by enabling the user to set proxyUser to clsadmin.
:   See [Livy API](/docs/AnalyticsEngine?topic=AnalyticsEngine-livy-api).

The default Python environment is now Python 3.
:   Earlier default Spark Python runtime was `Python 2`. See [Installed libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).

Added the Ambari configuration changes to the custom spark-default.conf file for Python 3 support.
:   See [Installed libraries](/docs/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).

Changes to installation library directory structure
:   Scala and R user installation library directory structure was changed to `~/scala` for Scala and `~/R` for R.

## November 2018
{: #AnalyticsEngine-nov18}


### 30 November 2018
{: #AnalyticsEngine-nov3018}
{: release-note}

You can now track life cycle management actions performed on the cluster by users and applications that have access to your service instance.
:   See [Activity Tracker](/docs/AnalyticsEngine?topic=AnalyticsEngine-at_events).

IAM role to view cluster password
:   To view the password of a cluster, a user must have either Writer or Manager IAM role on the {{site.data.keyword.iae_full_notm}} service instance.

### 09 November 2018
{: #AnalyticsEngine-nov0918}
{: release-note}

New {{site.data.keyword.iae_full_notm}} region
:   {{site.data.keyword.iae_full_notm}} is now also available in the region of Japan, in addition to the US-South, Germany, and United Kingdom regions.


    - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Japan is `https://api.jp-tok.ae.cloud.ibm.com`

    - An {{site.data.keyword.iae_full_notm}} cluster created in the region of Japan has the following format:

        ```
        <clustername>.jp-tok.ae.appdomain.cloud
        ```

        For example:

        `https://xxxxx-mn001.jp-tok.ae.appdomain.cloud:9443`

## September 2018
{: #AnalyticsEngine-sep18}


### 24 September 2018
{: #AnalyticsEngine-sep2418}
{: release-note}

New {{site.data.keyword.iae_full_notm}} region
:   {{site.data.keyword.iae_full_notm}} is now available in a new region namely Germany, in addition to US-South and the United Kingdom regions.
    - The {{site.data.keyword.iae_full_notm}} REST API endpoint for Germany is

        -	eu-de: `https://api.eu-de.ae.cloud.ibm.com`

The {{site.data.keyword.iae_full_notm}} cluster is now created using a new domain name to align with {{site.data.keyword.Bluemix_short}} domain name standards.
:   It has the following format:

    ```
    <clustername>.<region>.ae.appdomain.cloud
    ```

    where region is us-south, eu-gb, or eu-de.

    For example, for Germany:  
    `https://xxxxx-mn001.eu-de.ae.appdomain.cloud:9443`

    **Note:** Old clusters can still exist and will function using the old cluster name format.

Fixed quick links in Ambari UI
:   Broken HBase quick links are fixed on the Ambari UI.

Fixed Spark History links in Ambari UI
:   Broken stderr/stdout links are fixed for Spark History on the Ambari UI.


### 14 September 2018
{: #AnalyticsEngine-sep1418}
{: release-note}

Support for advanced provisioning options to customize Ambari component configurations at the time the {{site.data.keyword.iae_full_notm}} service instance is created was added.
:   See [Advanced provisioning options](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options).

New location for REST API documentation
:   The {{site.data.keyword.iae_full_notm}} REST API documentation can now be accessed at the following new location: `https://{DomainName}/apidocs/ibm-analytics-engine`

New domain suffix for endpoints
:   The {{site.data.keyword.iae_full_notm}} REST API endpoints have a new domain suffix:

    - us-south: `https://api.us-south.ae.cloud.ibm.com`
    - eu-gb: `https://api.eu-gb.ae.cloud.ibm.com`

    The older endpoints `https://api.dataplatform.ibm.com` and `https://api.eu-gb.dataplatform.ibm.com` are deprecated and will no longer be supported after the end of September 2018. You can create  new service credentials from the {{site.data.keyword.Bluemix_short}} console to fetch the new endpoint.

CRON jobs
:   You can set up cron jobs on the mn003 node.

    **Restriction**: You can set this up only on the mn003 node.

Support for Spark SQL JDBC endpoint was added.
:   See [Working with Spark SQL to query data](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-sql).

Stocator library updated
:   The Stocator library was updated to 1.0.24.

Fixed broken links
:   Broken Spark History links were fixed.

Optimized performance of Cloud Object Storage workloads
:   Changes were made at the backend networking level for optimized performance of Cloud Object Storage workloads.

Documented more examples for using HBase and Parquet-Hive.
:   See [Working with HBase](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hbase#moving-data-between-the-cluster-and-object-storage) and [Working with Hive](/docs/AnalyticsEngine?topic=AnalyticsEngine-working-with-hive#parquet).

Added the {{site.data.keyword.iae_full_notm}} security model.
:   See [Security model](/docs/AnalyticsEngine?topic=AnalyticsEngine-security-model).

Documented best practices for creating and maintaining a stateless cluster.
:   See [Best practices](/docs/AnalyticsEngine?topic=AnalyticsEngine-best-practices).

## 6 June 2018
{: #AnalyticsEngine-jun18}
{: release-note}

You can no longer provision {{site.data.keyword.iae_full_notm}} service instances using the Cloud Foundry CLI.
:   You must use the {{site.data.keyword.Bluemix_short}} CLI.

New software packages
:   The following software packages were added:

    - **AE 1.1 Spark**
    - **AE 1.1 Spark and Hadoop**
    - **AE 1.1 Spark and Hive**

    These packages are based on Hortonworks Data Platform 2.6.5 and  include Spark 2.3.


## 26 March 2018
{: #AnalyticsEngine-mar18}
{: release-note}

New {{site.data.keyword.iae_full_notm}} region
:   {{site.data.keyword.iae_full_notm}} service instances can now also be deployed in the United Kingdom region

Support for Resource Controller APIs and CLI tools to create an {{site.data.keyword.iae_full_notm}} service instance has been added.
:   Resource controller based service instances help you to provide fine grained access control to other users when you want to share your service instance.

New software pack
:   The software pack **AE 1.0 Spark and Hive** has been introduced. Choose this pack if you intend to work with Hive and/or Spark work loads and do not require other Hadoop components offered in the **AE 1.0 Spark and Hadoop** pack.

## 12 January 2018
{: #AnalyticsEngine-mar18}
{: release-note}

{{site.data.keyword.iae_full_notm}} now supports Apache Phoenix 4.7.
:   With this support, you can query HBase through SQL.

New version of Jupyter Enterise Gateway added
:   Jupyter Enterprise Gateway 0.8.0 was added

## 1 November 2017
{: #AnalyticsEngine-nov0117}
{: release-note}

Introducing {{site.data.keyword.iae_full_notm}}
:   {{site.data.keyword.iae_full_notm}} is now generally available. {{site.data.keyword.iae_full_notm}} provides a flexible framework to develop and deploy analytics applications in Apache Hadoop and Apache Spark.

New: {{site.data.keyword.iae_full_notm}} is now GA and offers new service plans.
:   The plans are:

    - Lite
    - Standard-Hourly
    - Standard-Monthly

    You can also select to configure a memory intensive hardware type.

    For details on provisioning instances, see [Provisioning an {{site.data.keyword.iae_full_notm}} service instance](/docs/AnalyticsEngine?topic=AnalyticsEngine-provisioning-IAE).
