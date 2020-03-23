---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-20"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Key management by application
{: #key-management-application}

This topic describes how to manage  column encryption keys by application. It explains how to provide master keys and how to write and read encrypted data using these master keys.

## Providing master keys

To provide master keys:

1. Pass the explicit master keys, in the following format:

  ```
  parameter name: "encryption.key.list"
  parameter value: "<master key ID>:<master key (base64)> , <master key ID>:<master key (base64)>.."
  ```

 For example:
 ```
 sc.hadoopConfiguration.set("encryption.key.list" , "k1:iKwfmI5rDf7HwVBcqeNE6w== , k2:LjxH/aXxMduX6IQcwQgOlw== , k3:rnZHCxhUHr79Y6zvQnxSEQ==")
 ```
 The length of master keys before base64 encoding can be 16, 24 or 32 bytes (128, 192 or 256 bits).

## Writing encrypted data

To write encrypted data:

1. Specify which columns to encrypted, and which master keys to use:
  ```
  parameter name:  "encryption.column.keys"
  parameter value: "<master key ID>:<column>,<column>;<master key ID>:<column> .."
  ```
1. Specify the footer key:
  ```
  parameter name:  "encryption.footer.key"
  parameter value:  "<master key ID>"
  ```
  For example:
  ```
  dataFrame.write
  .option("encryption.footer.key" , "k1")
  .option("encryption.column.keys" , "k2:SSN,Address;k3:CreditCard")
  .parquet("<path to encrypted files>")
  ```
  **Note**:
 - `"<path to encrypted files>"` must contain the string `.encrypted` in the URL, for example `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.
 - If either the `"encryption.column.keys"` parameter or the  `"encryption.footer.key"` parameter is not set, an exception will be thrown.

## Reading encrypted data

The required metadata is stored in the encrypted Parquet files.

To read the encrypted data:

1. Provide the encryption keys:

  ```
  sc.hadoopConfiguration.set("encryption.key.list" , "k1:iKwfmI5rDf7HwVBcqeNE6w== , k2:LjxH/aXxMduX6IQcwQgOlw== , k3:rnZHCxhUHr79Y6zvQnxSEQ==")
  ```
1. Call the regular parquet read commands, such as:
  ```
  val dataFrame = spark.read.parquet("<path to encrypted files>")
  ```

  **Note**: `"<path to encrypted files>"` must contain the string `.encrypted` in the URL, for example `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.
