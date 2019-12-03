---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-16"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Determining the {{site.data.keyword.cos_short}} credentials for {{site.data.keyword.iae_full_notm}}
{: #cos-credentials-in-iae}

The {{site.data.keyword.cos_full_notm}} access credentials are configured as key-value pair parameters in {{site.data.keyword.iae_full_notm}}.
These parameters start with the prefix `fs.cos.` followed by an identifier, which you can define, and end with the name of the credential.

Note that you can configure one {{site.data.keyword.iae_full_notm}} service instance to work with multiple {{site.data.keyword.cos_short}} instances or multiple {{site.data.keyword.cos_short}} credentials. To differentiate between the different sets of credentials, you use separate identifiers or literals, which you can defined.

The following diagram shows how the {{site.data.keyword.cos_short}} credentials are used in {{site.data.keyword.iae_full_notm}}.

![Authentication to {{site.data.keyword.cos_short}} resources from {{site.data.keyword.iae_full_notm}}](images/cos-credentials-in-iae.png)

The diagram shows two {{site.data.keyword.cos_short}} instances with two different data buckets `b1` and `b2`. To distinguish between the two instances, two identifiers are used, `firstbucket` and `secondbucket`. Note that the bucket names (`b1` and `b2`) that you use must be the actual names of the existing buckets; however you are free to choose any identifier that makes it easier for you to distinguish between {{site.data.keyword.cos_short}} instances. For example, as identifier, you can use `myinstance`, `productioninstance`, `joesinstance` and so on.

## Structure HMAC credentials
For HMAC style authentication, you must define the following parameters in {{site.data.keyword.iae_full_notm}}:
```
fs.cos.<identifier>.access.key=<Access Key ID>
fs.cos.<identifier>.endpoint=<EndPoint URL>
fs.cos.<identifier>.secret.key=<Secret Access Key>
```

Note that the endpoint URL has the format:
```
s3.private.us.cloud-object-storage.appdomain.cloud
```
It doesn't require the prefix `https://`.

## Structure of API key credentials

For IAM API key style authentication, you must define the following parameters in {{site.data.keyword.iae_full_notm}}:
```
fs.cos.<identifier>.iam.api.key=<IAM API Key>
fs.cos.<identifier>.endpoint=<EndPoint URL>
```

Like for HMAC credentials, the endpoint URL has the format:
```
s3.private.us.cloud-object-storage.appdomain.cloud
```
and doesn't require the prefix `https://`.

## Next steps

- Use one of the 3 available methods to configure the cluster to use the {{site.data.keyword.cos_short}} credentials. See [Configuring {{site.data.keyword.iae_full_notm}} to use   {{site.data.keyword.cos_short}}](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-configure-iae-with-cos).
