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

    - Directly by your application. See [Key management by application](/docs/AnalyticsEngine?topic=AnalyticsEngine-key-management-application).
    - By {{site.data.keyword.keymanagementservicefull}}, a centralized key management system (KMS) for generating, managing, and destroying encryption keys used by {{site.data.keyword.iae_full_notm}}. See [Key management by Key Protect](/docs/AnalyticsEngine?topic=AnalyticsEngine-key-management-key-protect).

     {{site.data.keyword.keymanagementservicefull}} helps you manage your encrypted keys by aligning with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles.

      **Note**: Only master encryption keys (MEKs) need to be managed (either by your application, or by Key Protect). Data encryption keys (DEKs) are automatically handled by Spark/Parquet. For details on key handling inside Parquet encryption, see  [Internals of encryption key handling](#internals-of-encryption-key-handling).

    For each sensitive column, you must specify which master key to use for encryption. Also, a master key must be specified for the footer of each encrypted file (data frame) - because the footers keep metadata like the schema and list of sensitive columns, that can be sensitive too. By default, the footer key will be used for footer encryption. However, if you choose a plain text footer mode, the footer won’t be encrypted, and the key will be used only for integrity verification of the footer.

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
8. You can use Spark to automatically encrypt content saved in Hive tables by storing the information about which columns to encrypt with which keys in the Apache Hive Metastore. See [Configuring Parquet encryption on Apache Hive tables](/docs/AnalyticsEngine?topic=AnalyticsEngine-parquet-encryption-on-hive-tables).

## Running {{site.data.keyword.iae_full_notm}} with Parquet encryption

To enable Parquet encryption in {{site.data.keyword.iae_full_notm}}, set the following Spark classpath properties to point to the Parquet jar files that implement Parquet modular encryption, and to the key management jar file:

1. Navigate to **Ambari > Spark > Config -> Custom spark2-default**.
1. Add the following two parameters to point explicitly to the location of the JAR files.

 Alternatively, you can get the JAR files applied as part of the cluster creation process. See [Advanced Provisioning](/docs/AnalyticsEngine?topic=AnalyticsEngine-advanced-provisioning-options){: external}.

 ```
 spark.driver.extraClassPath=/home/common/lib/parquetEncryption/ibmparquetkms.jar:/home/common/lib/parquetEncryption/parquetformat.jar:/home/common/lib/parquetEncryption/parquethadoop.jar
 spark.executor.extraClassPath=/home/common/lib/parquetEncryption/ibmparquetkms.jar:/home/common/lib/parquetEncryption/parquetformat.jar:/home/common/lib/parquetEncryption/parquethadoop.jar
 ```

## Mandatory parameters

The following parameters are required for writing encrypted data:

  - List of columns to encrypt, with the master encryption keys:
    ```
    parameter name: "parquet.encryption.column.keys"
    parameter value: "<master key ID>:<column>,<column>;<master key ID>:<column>,.."
    ```
  - The footer key:
    ```
    parameter name: "parquet.encryption.footer.key"
    parameter value: "<master key ID>"
    ```
    For example:
    ```
    dataFrame.write
    .option("parquet.encryption.footer.key" , "k1")
    .option("parquet.encryption.column.keys" , "k2:SSN,Address;k3:CreditCard")
    .parquet("<path to encrypted files>")
    ```

    **Important**: If neither the `"parquet.encryption.column.keys"` parameter nor the `"parquet.encryption.footer.key"` parameter is set, the file will not be encrypted. If only one of these parameters is set, an exception is thrown, because these parameters are mandatory for encrypted files.  

## Optional parameters

The following optional parameters can be used when writing encrypted data:
- The encryption algorithm `AES-GCM-CTR`

  By default, Parquet encryption uses the `AES-GCM` algorithm that provides full protection against tampering with data and metadata in Parquet files. However, as Spark 2.3.0 runs on Java 8, which doesn’t support AES acceleration in CPU hardware (this was only added in Java 9), the overhead of data integrity verification can affect workload throughput in certain situations.

  To compensate this, you can switch off the data integrity verification support and write the encrypted files with the alternative algorithm `AES-GCM-CTR`, which verifies the integrity of the metadata parts only and not that of the data parts, and has a lower throughput overhead compared to the `AES-GCM` algorithm.

  ```
  parameter name: "parquet.encryption.algorithm"
  parameter value: "AES_GCM_CTR_V1"
  ```
- Plain text footer mode for legacy readers

  By default, the main Parquet metadata module (the file footer) is encrypted to hide the file schema and list of sensitive columns. However, you can decide not to encrypt the file footers in order to enable other Spark and Parquet readers (that don't yet support Parquet encryption) to read the unencrypted columns in the encrypted files. To switch off footer encryption, set the following parameter:

  ```
  parameter name: "parquet.encryption.plaintext.footer"
  parameter value: "true"
  ```
  **Important**: The `"parquet.encryption.footer.key"` parameter must also be specified in the plain text footer mode. Although the footer is not encrypted, the key is used to sign the footer content, which means that new readers could verify its integrity. Legacy readers are not affected by the addition of the footer signature.

## Usage examples
{: #usage-examples-parquet-encryption}

The following sample code snippets for Python and Scala show how to create data frames, written to encrypted parquet files, and read from encrypted parquet files.

- Python: Writing encrypted data
```python
from pyspark.sql import

 RowsquaresDF = spark.createDataFrame(
    sc.parallelize(range(1, 6))
    .map(lambda i: Row(int_column=i,  square_int_column=i ** 2)))

 sc._jsc.hadoopConfiguration().set("encryption.key.list",
    "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")

 encryptedParquetPath = "squares.parquet.encrypted"squaresDF.write\
   .option("parquet.encryption.column.keys", "key1:square_int_column")\
   .option("parquet.encryption.footer.key", "key2")\
   .parquet(encryptedParquetPath)
```

- Python: Reading encrypted data
```python
sc._jsc.hadoopConfiguration().set("parquet.encryption.key.list",
     "key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")

 encryptedParquetPath = "squares.parquet.encrypted"
 parquetFile = spark.read.parquet(encryptedParquetPath)
 parquetFile.show()
```
- Scala: Writing encrypted data
```scala
case class SquareItem(int_column: Int, square_int_column: Double)
val dataRange = (1 to 6).toList
val squares = sc.parallelize(dataRange.map(i => new SquareItem(i, scala.math.pow(i,2))))
sc.hadoopConfiguration.set("parquet.encryption.key.list","key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")
val encryptedParquetPath = "squares.parquet.encrypted"
squares.toDS().write
  .option("parquet.encryption.column.keys", "key1:square_int_column")
  .option("parquet.encryption.footer.key", "key2")
  .parquet(encryptedParquetPath)
```

- Scala: Reading encrypted data
```scala
sc.hadoopConfiguration.set("parquet.encryption.key.list","key1: AAECAwQFBgcICQoLDA0ODw==, key2: AAECAAECAAECAAECAAECAA==")
val encryptedParquetPath = "squares.parquet.encrypted"
val parquetFile = spark.read.parquet(encryptedParquetPath)
parquetFile.show()
```

## Internals of encryption key handling
{: #encryption-internals}

When writing a Parquet file, a random data encryption key (DEK) is generated for each encrypted column and for the footer. These  keys are used to encrypt the data and the metadata modules in the Parquet file.

The data encryption key is then encrypted with a key encryption key (KEK), also generated inside Spark/Parquet for each master key. The key encryption key is encrypted with a master encryption key (MEK), either locally if the master keys are managed by the application, or in a KeyProtect service if the master keys are managed by {{site.data.keyword.keymanagementservicefull}}.

Encrypted data encryption keys and key encryption keys are stored in the Parquet file metadata, along with the master key identity. Each key encryption key has a unique identity (generated locally as a secure random 16-byte value), also stored in the file metadata. Key encryption keys are cached by access token, so there is no need to interact with the KeyProtect service for each encrypted column or file if they use the same master encryption key.

When reading a Parquet file, the identifier of the master encryption key (MEK) and the encrypted key encryption key (KEK) with its identifier, and the encrypted data encryption key (DEK)  are extracted from the file metadata.

The key encryption key is decrypted with the master encryption key, either locally if the master keys are managed by the application, or in a KeyProtect service if the master keys are managed by {{site.data.keyword.keymanagementservicefull}}. The key encryption keys are cached by access token, so there is no need to interact with the KeyProtect service for each decrypted column or file if they use the same key encryption key. Then the data encryption key (DEK) is decrypted locally, using the key encryption key (KEK).

## Key Rotation (optional feature)
{: #key-rotation}

Envelope encryption practice has an advanced option to *rotate* (change) master keys to minimize the risk of sensitive data leakage due to compromised MEKs. DEKs can’t be changed, because this means encrypting the Parquet files entirely again. The new MEKs (and transparently re-generated KEKs) are used to re-encrypt the DEKs.

To enable key rotation in Parquet files, you must set the `"parquet.encryption.key.material.store.internally"` parameter to `"false"` in the Hadoop configuration when writing the parquet files.

With this parameter set, Parquet stores the keys (wrapped with MEKs) in separate small files, instead of inside the Parquet files. When  key rotation is performed, only the small key material files are replaced. No need to modify the Parquet files (which are often immutable). Most KMS systems support key rotation by replacing the contents of the stored master keys and keeping the MEK ID intact (but updating the key version). After a user or administrator has performed the master key rotation inside the KMS instance using the KMS-specific API, the Parquet key rotation mechanism can be triggered (for example, by the same administrator) by calling:
```
public static void KeyToolkit.rotateMasterKeys(String folderPath, Configuration hadoopConfig)
```
When key rotation is performed, every KEK in all key material files in `folderPath` is unwrapped with the old MEK version to enable decrypting the DEK. A new KEK is generated for each master key ID, and wrapped with the new MEK version. Creating a new KEK, instead of re-using the old one, doesn't only mean that security is improved but also that the performance of subsequent data read operations is increased. This is because a single key rotation process, run by the administrator, operates across multiple files created by different processes, meaning many different KEKs for the same MEK ID. The key rotation process replaces the old KEKs with a single new KEK, which allows the readers to run a single unwrap interaction with KMS for each master key.

For examples of how to use key rotation:
- For key management by application, see [Key rotation in key management by application](/docs/AnalyticsEngine?topic=AnalyticsEngine-key-management-application#key-rotation-key-mgt-application).
- For key management by Key Protect, see [Key rotation key management by Key Protect](/docs/AnalyticsEngine?topic=AnalyticsEngine-key-management-key-protect#key-rotation-key-mgt-keyprotect).

## Learn more
{: #learn-more-parquet-encryption}

Check out the following sample notebook to learn how to use Parquet Encryption:

- [Parquet Encryption by Key Management Application](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/013c690997e27f3a8d91332653054441){: external}
