---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Key management by Key Protect
{: #key-management-key-protect}

This topic describes managing column encryption keys by using {{site.data.keyword.keymanagementservicefull}} (Key Protect). It explains how to create a Key Protect instance and to provide master keys and how to write and read encrypted data using these master keys.

### Creating a Key Protect instance and master keys

To create a Key protect instance and master keys:

1. Create a Key Protect service instance. See [Provisioning the service](/docs/key-protect?topic=key-protect-provision){: external}.
1. Create customer root keys in this instance. The customer root keys serve as master keys for column encryption. See [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys){: external}.

    If you want to use existing root keys, you can import those. See [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys){: external}.
1. Configure user access rights to the master keys by using the IBM IAM service. See [Granting access to master keys](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level){: external}.

## Writing encrypted data

To write encrypted data:

1. Pass the following parameters to IBM Analytics Engine:

    -	`"parquet.encryption.kms.instance.id"`: The ID of your KeyProtect instance, for example:
        ```
        sc.hadoopConfiguration.set("parquet.encryption.kms.instance.id" , "27861a9a-6779-4026-bca4-01e59acf0767")
        ```
    - `"parquet.encryption.kms.instance.url"`: The URL of your KeyProtect instance, for example:
        ```
        sc.hadoopConfiguration.set("parquet.encryption.kms.instance.url" , "https://<region>.kms.cloud.ibm.com")
        ```
    - `"parquet.encryption.key.access.token"`: A valid IAM token with access rights to the required keys in your KeyProtect instance, for example:
        ```
        sc.hadoopConfiguration.set("parquet.encryption.key.access.token" , "<token string>")
        ```
        If you keep the token in a local file, you can load it.
        ```
        val token = scala.io.Source.fromFile("<token file>").mkString
        sc.hadoopConfiguration.set("parquet.encryption.key.access.token" , token)
        ```
1. Specify which columns need to be encrypted, and with which master keys. You must also specify the footer key. In key management by Key Protect, the master key IDs are the IDs of the Key Protect CRKs (customer root keys), that you can find on the IBM Cloud service window. For example:

    ```
    val k1 = "d1ae3fc2-6b7d-4a03-abb6-644e02933734"
    val k2 = "c4a21521-2a78-4968-a7c2-57c481f58d5c"
    val k3 = "a4ae4bc2-9d78-8748-f8a2-17f584d48c5b"

    dataFrame.write
    .option("parquet.encryption.footer.key" , k1)
    .option("parquet.encryption.column.keys" , k2+":SSN,Address;"+k3+":CreditCard")
    .parquet("<path to encrypted files>")
    ```
    **Note**: If either the `"parquet.encryption.column.keys"` parameter or the  `"parquet.encryption.footer.key"` parameter is not set, an exception will be thrown.

## Reading encrypted data

The required metadata, including the ID and URL of the KeyProtect instance, is stored in the encrypted Parquet files.

To read the encrypted metadata:
1. Provide the IAM access token for the relevant keys:
    ```
    sc.hadoopConfiguration.set("parquet.encryption.key.access.token" , "<token string>")
    ```
1. Call the regular parquet read commands, such as:
    ```
    val dataFrame = spark.read.parquet("<path to encrypted files>")
    ```

## Key rotation
{: #key-rotation-key-mgt-keyprotect}

If key rotation is required, the administrator has to rotate master keys in Key Protect using the procedure described in [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys){: external}. Then the administrator can trigger Parquet key rotation by calling:

```
public static void KeyToolkit.rotateMasterKeys(String folderPath, Configuration hadoopConfig)
```

To enable Parquet key rotation, the following Hadoop configuration properties must be set:
- The parameters `"parquet.encryption.key.access.token"`, `"parquet.encryption.kms.instance.url"`, `"parquet.encryption.kms.instance.id"`
- The parameter `"parquet.encryption.key.material.store.internally"` must be set to `"false"`
- The parameter `"parquet.encryption.kms.client.class"` must be set to `"com.ibm.parquet.key.management.KeyProtectClient"`

For example:
```
sc.hadoopConfiguration.set("parquet.encryption.kms.instance.id", "27861a9a-6779-4026-bca4-01e59acf0767")
sc.hadoopConfiguration.set("parquet.encryption.kms.instance.url" , "https://<region>.kms.cloud.ibm.com")
sc.hadoopConfiguration.set("parquet.encryption.key.access.token" , "<token string>")
sc.hadoopConfiguration.set("parquet.encryption.kms.client.class" "com.ibm.parquet.key.management.KeyProtectClient")
sc.hadoopConfiguration.set("parquet.encryption.key.material.store.internally", "false")

KeyToolkit.rotateMasterKeys("<path to encrypted files>", sc.hadoopConfiguration)
```
