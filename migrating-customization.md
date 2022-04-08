---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-01"

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

# Migrating from classic instances: customization
{: #migrate-customization}

This topic helps you when migrating from classic to serverless instances by providing a brief summary of the differences and sample code snippets when customizing {{site.data.keyword.iae_short}} instances.

## Customizing instances

The following table summarizes the differences when customizing classic and serverless instances.

| Classic instances     | Serverless instances |
|-----------------------|--------------------------|
| - Customization is at cluster level   \n - Supports bootstrap customization and adhoc customization   \n - Supports customization that executes a shell script with any user given commands   \n - Supports the `package-admin` tool to install, update, or remove operating system packages from the `centOS` repository  \n - Supported customization script are hosted on Github and `https` locations | - Customization is at instance level, which means that the customization is available for all Spark workloads against that instance   \n - Supports "adhoc" customization - that is, outside of instance creation   \n - Supports Python based script customization   \n - No support for the `package-admin` tool  \n - Supports {{site.data.keyword.cos_short}} based location of scripts |

## Customization examples

The following examples show how you can add custom Python or R packages in classic instances and what the equivalent is in serverless instances.

- Install desired packages:

    For classic instances, submit a customization script that installs the file package using:
    ```
    pip install <package name>
    ```
    For serverless instances, create a library set with the desired packages, then submit the Spark application and reference the library set. Note that currently only Python library sets are supported.
    ```
    {
        "library_set": {
            "action": "add",
            "name": "my_library_set", 
            "libraries": {
                "conda": {
                    "python": {
                        "packages": ["numpy"]
                    }
                }
            }
        }
    }
    ```

    For details, see [Creating a library set](/docs/AnalyticsEngine?topic=AnalyticsEngine-create-lib-set).
    
- Download desired packages:

    For classic instances, submit a customization script that would download a file into cluster nodes (local file system) using either: 
    ```
    wget <file url>
    ```
    or:
    ```
    hadoop fs -get s3a://mybucket.myservice/myobject
    ```
    For serverless instances, use of customization script that will download the file you require into the cluster into a "libraryset" directory and then refrence the library set when you submit the Spark application under the `"ae.spark.librarysets"` element.
    
    For details, see [Using a library set](/docs/AnalyticsEngine?topic=AnalyticsEngine-cust-script).
    
