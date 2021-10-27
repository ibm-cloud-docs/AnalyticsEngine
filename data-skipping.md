---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-24"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:note: .note}
{:hint: .hint}
{:important: .important}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Data skipping for Spark SQL
{: #data-skipping}

Data skipping can significantly boost the performance of SQL queries by skipping over irrelevant data objects or files based on a summary metadata associated with each object.

Data skipping uses the open source Xskipper library for creating, managing and deploying data skipping indexes with Apache Spark. See [Xskipper - An Extensible Data Skipping Framework](https://xskipper.io){: external}.

For more details on how to work with Xskipper see:
- [Quick Start Guide](https://xskipper.io/getting-started/quick-start-guide/){: external}
- [Demo Notebooks](https://xskipper.io/getting-started/sample-notebooks/){: external}

In addition to the open source features in Xskipper, the following features are also available:
- [Geospatial data skipping](#geospatial-skipping)
- [Encrypting indexes](#encrypting-indexes)
- [Samples showing these features](#samples)

## Geospatial data skipping
{: #geospatial-skipping}

You can also use data skipping when querying geospatial data sets using [geospatial functions](https://www.ibm.com/support/knowledgecenter/en/SSCJDQ/com.ibm.swg.im.dashdb.analytics.doc/doc/geo_functions.html){: external} from the [spatio-temporal library](/docs/AnalyticsEngine?topic=AnalyticsEngine-geospatial-geotemporal-lib){: external}.

- To benefit from data skipping in data sets with latitude and longitude columns, you can collect the min/max indexes on the latitude and longitude columns.
- Data skipping can be used in data sets with a geometry column (a UDT column) by using a built-in [Xskipper plugin](https://xskipper.io/api/indexing/#plugins){: external}.

The next sections show you to work with the geospatial plugin.

### Setting up the geospatial plugin

To use a plugin, load the relevant implementations using the Registration module:

- For Scala:
    ```scala
    import com.ibm.xskipper.stmetaindex.filter.STMetaDataFilterFactory
    import com.ibm.xskipper.stmetaindex.index.STIndexFactory
    import com.ibm.xskipper.stmetaindex.translation.parquet.{STParquetMetaDataTranslator, STParquetMetadatastoreClauseTranslator}
    import io.xskipper._

    Registration.addIndexFactory(STIndexFactory)
    Registration.addMetadataFilterFactory(STMetaDataFilterFactory)
    Registration.addClauseTranslator(STParquetMetadatastoreClauseTranslator)
    Registration.addMetaDataTranslator(STParquetMetaDataTranslator)
    ```

- For Python:
    ```python
    from xskipper import Xskipper
    from xskipper import Registration

    Registration.addMetadataFilterFactory(spark, 'com.ibm.xskipper.stmetaindex.filter.STMetaDataFilterFactory')
    Registration.addIndexFactory(spark, 'com.ibm.xskipper.stmetaindex.index.STIndexFactory')
    Registration.addMetaDataTranslator(spark, 'com.ibm.xskipper.stmetaindex.translation.parquet.STParquetMetaDataTranslator')
    Registration.addClauseTranslator(spark, 'com.ibm.xskipper.stmetaindex.translation.parquet.STParquetMetadatastoreClauseTranslator')
    ```

### Index building

To build an index, you can use the `addCustomIndex` API:

- For Scala:
    ```scala
    import com.ibm.xskipper.stmetaindex.implicits._

    // index the dataset
    val xskipper = new Xskipper(spark, dataset_path)

    xskipper
      .indexBuilder()
      // using the implicit method defined in the plugin implicits
      .addSTBoundingBoxLocationIndex("location")
      // equivalent
      //.addCustomIndex(STBoundingBoxLocationIndex("location"))
      .build(reader).show(false)
    ```

- For Python:
    ```python
    xskipper = Xskipper(spark, dataset_path)

    # adding the index using the custom index API
    xskipper.indexBuilder() \
            .addCustomIndex("com.ibm.xskipper.stmetaindex.index.STBoundingBoxLocationIndex", ['location'], dict()) \
            .build(reader) \
            .show(10, False)
    ```

### Supported functions

The list of supported geospatial functions includes the following:

- ST_Distance
- ST_Intersects
- ST_Contains
- ST_Equals
- ST_Crosses
- ST_Touches
- ST_Within
- ST_Overlaps
- ST_EnvelopesIntersect
- ST_IntersectsInterior

## Encrypting indexes
{: #encrypting-indexes}

If you use a Parquet metadata store, the metadata can optionally be encrypted using Parquet Modular Encryption (PME). This is achieved by storing the metadata itself as a Parquet data set, and thus PME can be used to encrypt it. This feature applies to all input formats, for example, a data set stored in CSV format can have its metadata encrypted using PME.

In the following section, unless specified otherwise, when referring to footers, columns, and so on, these are with respect to metadata objects, and not to objects in the indexed data set.

Index encryption is modular and granular in the following way:
- Each index can either be encrypted (with a per-index key granularity) or left plaintext
- Footer + object name column:
    - Footer column of the metadata object which in itself is a Parquet file contains, among other things:
        - Schema of the metadata object, which reveals the types, parameters and column names for all indexes collected. For  example, you can learn that a `BloomFilter` is defined on column `city` with a false-positive probability of `0.1`.
        - Full path to the original data set or a table name in case of a Hive metastore table.
    - Object name column stores the names of all indexed objects.
- Footer + metadata column can either be:
    - Both encrypted using the same key. This is the default. In this case, the plain text footer configuration for the Parquet objects comprising the metadata in encrypted footer mode, and the object name column is encrypted using the selected key.
    - Both in plain text. In this case, the Parquet objects comprising the metadata are in plain text footer mode, and the object name column is not encrypted.

      If at least one index is marked as encrypted, then a footer key must be configured regardless of whether plain text footer mode is enabled or not. If plain text footer is set then the footer key is used only for tamper-proofing. Note that in that case the object name column is not tamper  proofed.

      If a footer key is configured, then at least one index must be encrypted.

Before using index encryption, you should check the documentation on [PME](/docs/AnalyticsEngine?topic=AnalyticsEngine-parquet-encryption) and make sure you are familiar with the concepts.

When using index encryption, whenever a `key` is configured in any Xskipper API, it's always the label `NEVER the key itself`.
{: important}

To use index encryption:

1. Follow all the steps to make sure PME is enabled. See [PME](/docs/AnalyticsEngine?topic=AnalyticsEngine-parquet-encryption){: new_window}.
1. Perform all *regular* PME configurations, including Key Management configurations.
1. Create encrypted metadata for a data set:

    1. Follow the regular flow for creating metadata.
    1. Configure a footer key. If you wish to set a plain text footer + object name column, set `io.xskipper.parquet.encryption.plaintext.footer` to `true` (See samples below).
    1. In `IndexBuilder`, for each index you want to encrypt, add the label of the key to use for that index.

    To use metadata during query time or to refresh existing metadata, no setup is necessary other than the *regular* PME setup required to make sure the keys are accessible (literally the same configuration needed to read an encrypted data set).
    {: hint}

## Samples
{: #samples}

The following samples show metadata creation using a key named `k1` as a footer + object name key, and a key named `k2` as a key to encrypt a `MinMax` for `temp`, while also creating a `ValueList` for `city`, which is left in plain text.

- For Scala:
    ```scala
    // index the dataset
    val xskipper = new Xskipper(spark, dataset_path)
    // Configuring the JVM wide parameters
    val jvmComf = Map(
      "io.xskipper.parquet.mdlocation" -> md_base_location,
      "io.xskipper.parquet.mdlocation.type" -> "EXPLICIT_BASE_PATH_LOCATION")
    Xskipper.setConf(jvmConf)
    // set the footer key
    val conf = Map(
      "io.xskipper.parquet.encryption.footer.key" -> "k1")
    xskipper.setConf(conf)
    xskipper
      .indexBuilder()
      // Add an encrypted MinMax index for temp
      .addMinMaxIndex("temp", "k2")
      // Add a plaintext ValueList index for city
      .addValueListIndex("city")
      .build(reader).show(false)
      ```

- For Python
    ```python
    xskipper = Xskipper(spark, dataset_path)
    # Add JVM Wide configuration
    jvmConf = dict([
    ("io.xskipper.parquet.mdlocation", md_base_location),
    ("io.xskipper.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")])
    Xskipper.setConf(spark, jvmConf)
    # configure footer key
    conf = dict([("io.xskipper.parquet.encryption.footer.key", "k1")])
    xskipper.setConf(conf)
    # adding the indexes
    xskipper.indexBuilder() \
            .addMinMaxIndex("temp", "k1") \
            .addValueListIndex("city") \
            .build(reader) \
            .show(10, False)
    ```

If you want the footer + object name to be left in plain text mode (as mentioned above), you need to add the configuration parameter:

- For Scala:
    ```scala
    // index the dataset
    val xskipper = new Xskipper(spark, dataset_path)
    // Configuring the JVM wide parameters
    val jvmComf = Map(
      "io.xskipper.parquet.mdlocation" -> md_base_location,
      "io.xskipper.parquet.mdlocation.type" -> "EXPLICIT_BASE_PATH_LOCATION")
    Xskipper.setConf(jvmConf)
    // set the footer key
    val conf = Map(
      "io.xskipper.parquet.encryption.footer.key" -> "k1",
      "io.xskipper.parquet.encryption.plaintext.footer" -> "true")
    xskipper.setConf(conf)
    xskipper
      .indexBuilder()
      // Add an encrypted MinMax index for temp
      .addMinMaxIndex("temp", "k2")
      // Add a plaintext ValueList index for city
      .addValueListIndex("city")
      .build(reader).show(false)
      ```

- For Python
    ```python
    xskipper = Xskipper(spark, dataset_path)
    # Add JVM Wide configuration
    jvmConf = dict([
    ("io.xskipper.parquet.mdlocation", md_base_location),
    ("io.xskipper.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")])
    Xskipper.setConf(spark, jvmConf)
    # configure footer key
    conf = dict([("io.xskipper.parquet.encryption.footer.key", "k1"),
    ("io.xskipper.parquet.encryption.plaintext.footer", "true")])
    xskipper.setConf(conf)
    # adding the indexes
    xskipper.indexBuilder() \
            .addMinMaxIndex("temp", "k1") \
            .addValueListIndex("city") \
            .build(reader) \
            .show(10, False)
    ```

## Support for older metadata

Xskipper supports older metadata created by the MetaIndexManager seamlessly. Older metadata can be used for skipping as updates to the Xskipper metadata are carried out automatically by the next refresh operation.

If you see `DEPRECATED_SUPPORTED` in front of an index when listing indexes or running a `describeIndex` operation, the metadata version is deprecated but is still supported and skipping will work. The next refresh operation will update the metadata automatically.
