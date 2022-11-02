---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-30"

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

# Working with Spark SQL and an external metastore 
{: #external-metastore}

Spark SQL uses Hive metastore to manage the metadata of a user's applications tables, columns, partition information. 

By default, the database that powers this metastore is an embedded Derby instance that comes with the Spark cluster.  You could choose to externalize this metastore database to an external data store, like to an {{site.data.keyword.databases-for-postgresql_full_notm}} or an {{site.data.keyword.sqlquery_notm}} (previously SQL Query) instance. 

Placing your metadata outside of the Spark cluster will enable you to reference the tables in different applications across your {{site.data.keyword.iae_full_notm}} instances. This, in combination with storing your data in {{site.data.keyword.cos_full_notm}}, helps persisting data and metadata and allows you to work with this data seamlessly across different Spark workloads.

## Enabling and testing an external metastore with {{site.data.keyword.iae_full_notm}}
{: #test-external-metastore-with-iae}

To enable and test an external metastore with {{site.data.keyword.iae_full_notm}}, you need to perform the following steps:

1. Create a metastore to store the metadata. You can choose to provision either an {{site.data.keyword.databases-for-postgresql_full_notm}} or an {{site.data.keyword.sqlquery_notm}} (previously SQL Query) instance.
1. Configure {{site.data.keyword.iae_full_notm}} to work with the database instance. 
1. Create a table in one Spark application and then access this table from another Spark application.



