---

copyright:
  years: 2017, 2020
lastupdated: "2019-01-07"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Working with Parquet encryption
{: #parquet-encryption}

{{site.data.keyword.iae_full_notm}} supports Parquet modular encryption, a new addition to the Parquet standard that allows encrypting sensitive columns when writing Parquet files, and decrypting these columns when reading the encrypted files. See [Parquet modular encryption](https://github.com/apache/parquet-format/blob/apache-parquet-format-2.7.0/Encryption.md){: external}.

Besides ensuring privacy, Parquet encryption also protects the integrity of stored data. Any tampering with file contents is  detected and triggers a reader-side exception.

Key features include:

1. Parquet encryption and decryption is performed in the Spark workers. Therefore, sensitive data and the encryption keys are not visible to the storage.
2. Standard Parquet features, such as encoding, compression, columnar projection and predicate push-down, continue to work as usual on files with Parquet modular encryption format.
3. You can choose one of the two encryption algorithms that are defined in the Parquet specification. Both algorithms support column encryption, however:

  - The default algorithm `AES-GCM` provides full protection against tampering with data and metadata parts in Parquet files.
  - The alternative algorithm `AES-GCM-CTR` supports partial integrity protection of Parquet files. Only metadata parts are protected against tampering, not data parts. An advantage of this algorithm is that it has a lower throughput overhead compared to the `AES-GCM` algorithm.
4. You can choose which columns to encrypt. Other columns won’t be encrypted, reducing the throughput overhead.
5. Different columns can be encrypted with different keys.
6. By default, the main Parquet metadata module (the file footer) is encrypted to hide the file schema and list of sensitive columns. However, you can choose not to encrypt the file footers  in order to enable legacy readers (such as other Spark  distributions that don't yet support Parquet encryption) to read the unencrypted columns in the encrypted files.  
7. Encryption keys can be managed in one of two ways:

   - Directly by your application
   - By {{site.data.keyword.keymanagementservicefull}}, a centralized key management system (KMS) for generating, managing, and destroying encryption keys used by {{site.data.keyword.iae_full_notm}}.  {{site.data.keyword.keymanagementservicefull}} helps you manage your encrypted keys by aligning with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles.

   **Note**: Only master encryption keys (MEKs) need to be managed (either by your application, or by IBM KMS). Data encryption keys (DEKs) are automatically handled by Spark/Parquet. For details on key handling inside Parquet encryption, see  [Internals of encryption key handling](#internals-of-encryption-key-handling).

   For each sensitive column, you must specify which master key to use for encryption. Also, a master key must be specified for the footer of each encrypted file (data frame). By default, the footer key will be used for footer encryption. However, if you choose a plain text footer mode, the footer won’t be encrypted, and the key will be used only for integrity verification of the footer.

   The encryption parameters can be passed via the standard Spark Hadoop configuration, for example by setting configuration values in the Hadoop configuration of the application's SparkContext:
   ```
   sc.hadoopConfiguration.set("<parameter name>" , "<parameter value>")
   ```
   Alternatively, you can pass parameter values through write options:

   ```
   <data frame name>.write
   .option("<parameter name>" , "<parameter value>")
   .parquet("<write path>")
   ```

## Running {{site.data.keyword.iae_full_notm}} with Parquet encryption

To enable Parquet encryption in {{site.data.keyword.iae_full_notm}}, set the following Spark classpath properties to point to the Parquet jar files that implement Parquet modular encryption, and to the key management jar file:

1. Navigate to **Ambari > Spark > Config -> Custom spark2-default**.
1. Add the following two parameters to point explicitly to the location of the jar files. Make sure that you edit the paths to use the actual version of jar files on the cluster.

 Alternatively, you can get the jar files applied as part of the cluster creation process. See [Advanced Provisioning](/docs/services/key-protect?topic=advanced-provisioning-options){: external}.

 ```
 spark.driver.extraClassPath=/home/common/lib/parquet-encryption/ibm-parquet-kms-<latestversion>-jar-with-dependencies.jar:/home/common/lib/parquet-encryption/parquet-format-<latestversion>.jar:/home/common/lib/parquet-encryption/parquet-hadoop-<latestversion>.jar

 spark.executor.extraClassPath=/home/common/lib/parquet-encryption/ibm-parquet-<latestversion>-jar-with-dependencies.jar:/home/common/lib/parquet-encryption/parquet-format-<latestversion>.jar:/home/common/lib/parquet-encryption/parquet-hadoop-<latestversion>.jar
 ```

## Key management by application

This section shows you how to manage the column encryption keys  by application. It explains how to provide master keys and how to write and read encrypted data using these master keys.

### Providing master keys

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

### Writing encrypted data

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
 - `"<path to encrypted files>"` must contain the string `encrypted` in the URL, for example `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.
 - If either the `"encryption.column.keys"` parameter or the  `"encryption.footer.key"` parameter is not set, an exception will be thrown.

### Reading encrypted data

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

 **Note**: `"<path to encrypted files>"` must contain the string `encrypted` in the URL, for example `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.

## Key management by IBM KMS

This section shows you how to manage the column encryption keys  by using {{site.data.keyword.keymanagementservicefull}}. It explains how to create an IBM KMS instance and to provide master keys and how to write and read encrypted data using these master keys.

### Creating a KMS instance and master keys

To create a KMS instance and master keys:

1. Create an IBM KeyProtect service instance. See [Provisioning the service](https://cloud.ibm.com/docs/services/key-protect?topic=key-protect-provision){: external}.
1. Create customer root keys inside this instance. The customer root keys serve as master keys for column encryption. See [Creating root keys](/docs/services/key-protect?topic=key-protect-create-root-keys){: external}.

 If you want to use existing root keys, you can import those. See [Importing root keys](/docs/services/key-protect?topic=key-protect-import-root-keys){: external}.
1. Configure user access rights to the master keys by using the IBM IAM service. See [Granting access to master keys](/docs/services/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level){: external}.
1. Pass the following parameters to IBM Analytics Engine:

   - 	`"encryption.kms.instance.id"`: The ID of your KeyProtect instance, for example,
  ```
  sc.hadoopConfiguration.set("encryption.kms.instance.id" , "27861a9a-6779-4026-bca4-01e59acf0767")
  ```
  - `"encryption.kms.instance.url"`: The URL of your KeyProtect instance, for example,
  ```
  sc.hadoopConfiguration.set("encryption.kms.instance.url" , "https://$REGION.kms.cloud.ibm.com")
  ```
  - `"encryption.key.access.token"`: A valid IAM token with access rights to the required keys in your KeyProtect instance, for example,
  ```
  sc.hadoopConfiguration.set("encryption.key.access.token" , "<token string>")
  ```
  If you keep the token in a local file, you can load it:
  ```
  val token = scala.io.Source.fromFile("<token file>").mkString
  sc.hadoopConfiguration.set("encryption.key.access.token" , token)
  ```

### Writing encrypted data

To write encrypted data:

1. Specify which columns need to be encrypted, and with which master keys. You must also specify the footer key. In key management by IBM KMS, the master key IDs are the IDs of KeyProtect CRKs (customer root keys), that you can find on the IBM Cloud service window. For example:

  ```
  val k1 = "d1ae3fc2-6b7d-4a03-abb6-644e02933734"
  val k2 = "c4a21521-2a78-4968-a7c2-57c481f58d5c"
  val k3 = "a4ae4bc2-9d78-8748-f8a2-17f584d48c5b"

  dataFrame.write
  .option("encryption.footer.key" , k1)
  .option("encryption.column.keys" , k2+":SSN,Address;"+k3+":CreditCard")
  .parquet("<path to encrypted files>")
  ```
  **Note**:
  - `"<path to encrypted files>"` must contain the string `encrypted` in the URL, for example `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.
  - If either the `"encryption.column.keys"` parameter or the  `"encryption.footer.key"` parameter is not set, an exception will be thrown.

### Reading encrypted data

The required metadata, including the ID and URL of the KeyProtect instance, is stored in the encrypted Parquet files.

To read the encrypted metadata:
1. Provide the IAM access token for the relevant keys:
  ```
  sc.hadoopConfiguration.set("encryption.key.access.token" , "<token string>")
  ```
1. Call the regular parquet read commands, such as:
  ```
  val dataFrame = spark.read.parquet("<path to encrypted files>")
  ```
 **Note**: `"<path to encrypted files>"` must contain the string `encrypted` in the URL, for example, `"cos://<bucket>.<identifier>/my_table.parquet.encrypted"`.

## Optional parameters

The following optional parameters can be used when writing encrypted data:
- The encryption algorithm `AES-GCM-CTR`

 By default, Parquet encryption uses the `AES-GCM` algorithm that provides full protection against tampering with data and metadata in Parquet files. However, as Spark 2.3.0 runs on Java 8, which doesn’t support AES acceleration in CPU hardware (this was only added in Java 9), the overhead of data integrity verification can affect workload throughput in certain situations.

 To compensate this, you can switch off the data integrity verification support and write the encrypted files with the alternative algorithm `AES-GCM-CTR`, which verifies the integrity of the metadata parts only and not that of the data parts, and has a lower throughput overhead compared to the `AES-GCM` algorithm.

 ```
 parameter name: "encryption.algorithm"
 parameter value: "AES_GCM_CTR_V1"
 ```
- Plain text footer mode for legacy readers

 By default, the main Parquet metadata module (the file footer) is encrypted to hide the file schema and list of sensitive columns. However, you can decide not to encrypt the file footers in order to enable other Spark and Parquet readers (that don't yet support Parquet encryption) to read the unencrypted columns in the encrypted files. To switch off footer encryption, set the following parameter:

 ```
 parameter name: "encryption.plaintext.footer"
 parameter value: "true"
 ```

## Usage examples

The following sample code snippets for Python and Scala show how to create data frames, written to encrypted parquet files, and read from encrypted parquet files.

- Python: Writing encrypted data
```
from pyspark.sql import Row

 squaresDF = spark.createDataFrame(
     sc.parallelize(range(1, 6))
     .map(lambda i: Row(int_column=i,  square_int_column=i ** 2)))

 sc._jsc.hadoopConfiguration().set("encryption.key.list",
     "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")
 sc._jsc.hadoopConfiguration().set("encryption.column.keys",
     "key1:square_int_column")
 sc._jsc.hadoopConfiguration().set("encryption.footer.key", "key2")

 encryptedParquetPath = "squares.parquet.encrypted"
 squaresDF.write.parquet(encryptedParquetPath)
```
- Python: Reading encrypted data
```
sc._jsc.hadoopConfiguration().set("encryption.key.list",
     "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")

 encryptedParquetPath = "squares.parquet.encrypted"
 parquetFile = spark.read.parquet(encryptedParquetPath)
 parquetFile.show()
```
- Scala: Writing encrypted data
```
 case class SquareItem(int_column: Int, square_int_column: Double)
 val dataRange = (1 to 6).toList
 val squares = sc.parallelize(
     dataRange.map(i => new SquareItem(i, scala.math.pow(i,2))))

 sc.hadoopConfiguration.set("encryption.key.list",
   "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")
 sc.hadoopConfiguration.set("encryption.column.keys", "key1:square_int_column")
 sc.hadoopConfiguration.set("encryption.footer.key", "key2")

 val encryptedParquetPath = "squares.parquet.encrypted"
 squares.toDS().write.parquet(encryptedParquetPath)
```
- Scala: Reading encrypted data
```
 sc.hadoopConfiguration.set("encryption.key.list",
   "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")
 val encryptedParquetPath = "squares.parquet.encrypted"
 val parquetFile = spark.sqlContext.read.parquet(encryptedParquetPath)
parquetFile.show()
```

## Internals of encryption key handling

When writing a Parquet file, a random data encryption key (DEK) is generated for each encrypted column and for the footer. These  keys are used to encrypt the data and the metadata modules in the Parquet file.

The data encryption key is then encrypted with a key encryption key (KEK), also generated inside Spark/Parquet for each master key. The key encryption key is encrypted with a master encryption key (MEK), either locally if the master keys are managed by the application, or in a KeyProtect service if the master keys are managed by {{site.data.keyword.keymanagementservicefull}}.

Encrypted data encryption keys and key encryption keys are stored in the Parquet file metadata, along with the master key identity. Each key encryption key has a unique identity (generated locally as a secure random 16-byte value), also stored in the file metadata. Key encryption keys are cached, so there is no need to interact with the KeyProtect service for each encrypted column or file if they use the same master encryption key.

When reading a Parquet file, the identifier of the master encryption key (MEK) and the encrypted key encryption key (KEK) with its identifier, and the encrypted data encryption key (DEK)  are extracted from the file metadata.

The key encryption key is decrypted with the master encryption key, either locally if the master keys are managed by the application, or in a KeyProtect service if the master keys are managed by {{site.data.keyword.keymanagementservicefull}}. The key encryption keys are cached, so there is no need to interact with the KeyProtect service for each decrypted column or file if they use the same key encryption key.
