---

copyright:
  years: 2017, 20120
lastupdated: "2020-02-11"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Data skipping configuration options
{: #data-skipping-config-options}

The following configuration options are available:

| Key  | Default setting  | Description |
|------------|--------------|--------------|
|`spark.ibm.metaindex.`<br/>`evaluation.enabled`| `false` | When set to `true`, queries will run in evaluation mode. If queries run in evaluation mode, all of the indexed data sets will only be processed for skipping statistics and no data is read. Evaluation mode is useful to see the skipping statistics you might get for a given query. |
|`spark.ibm.metaindex.`<br/>`index.bloom.fpp`|	0.01 |The false/positive rate of the bloom filter. |
|`spark.ibm.metaindex.index.minmax.`<br/>`readoptimized.parquet` |	`true` |	If enabled, min/max statistics for Parquet objects on numerical columns (excluding decimal type) will be read from the footers. By collecting only the min/max indexes, index collection is further optimized by running with higher parallelism. |
|`spark.ibm.metaindex.index.minmax.`<br/>`readoptimized.parquet.parallelism`| 10000 |The parallelism to use when collecting only min/max indexes on Parquet files.|
|`spark.ibm.metaindex.`<br/>`parquet.mdlocation.type`| `EXPLICIT_BASE_PATH_LOCATION` |	The type of URL stored in the metadata location |
|`spark.ibm.metaindex.`<br/>`parquet.mdlocation`| `/tmp/dataskipping_metadata`| The metadata location (interpreted according to the URL type).|
|`spark.ibm.metaindex.`<br/>`parquet.index.chunksize`| 25000 | The number of objects to index in each chunk. |
|`spark.ibm.metaindex.`<br/>`parquet.maxRecordsPerFile`| 100000 | The maximum number of records per metadata file.|

## Types of metadata location
{: #metadata-location-types}

The parameter `spark.ibm.metaindex.parquet.mdlocation` is interpreted according to the URL type defined in the parameter `spark.ibm.metaindex.parquet.mdlocation.type`.

The following options are available:

- `EXPLICIT_BASE_PATH_LOCATION`: This is the default. An explicit definition of the base path to the metadata, which is combined with a data set identifier. This case can be used to configure the MetaIndexManager JVM wide settings and have all of data sets metadata saved under the base path.
- `EXPLICIT_LOCATION`: An explicit full path to the metadata.
- `HIVE_TABLE_NAME`: The name of the Hive table (in the form of `<db>.<table>`) that contains the exact path of the metadata in the table properties under the parameter `spark.ibm.metaindex.parquet.mdlocation`.
- `HIVE_DB_NAME`: The name of the Hive database that contains the base path of the metadata in the database properties under the parameter `spark.ibm.metaindex.parquet.mdlocation`.

You can set the `spark.ibm.metaindex.parquet.mdlocation` in two ways:

- Setting JVM wide configuration

  This configuration is useful for setting base location once for all data sets and should be used with the `EXPLICIT_BASE_PATH_LOCATION` or `HIVE_DB_NAME` types.

  The location of the metadata for each data set will be inferred automatically by combining the base path with a data set identifier.

  - For Scala:
  ```scala
    val jmap = new java.util.HashMap[String,String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/path/to/base/metadata/location")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")
    MetaIndexManager.setConf(jmap)
  ```

  - For Python:
  ```python
    md_backend_config = dict([
    ('spark.ibm.metaindex.parquet.mdlocation', "/path/to/base/metadata/location"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")])
    MetaIndexManager.setConf(spark, md_backend_config)
  ```

  Note that if the location does not exist in the Hive table, the following fallback is used:

  `HIVE_DB_NAME` (as base path) -> `/tmp/dataskipping_metadata` (as base path)

- Setting the metadata location for a specific MetaIndexManager instance

  This configuration is useful for setting a specific metadata location for a certain data set and should be used with the `EXPLICIT_LOCATION` or `HIVE_TABLE_NAME` type.

  - For Scala:
  ```scala
    val im = new MetaIndexManager(spark, uri, ParquetMetadataBackend)
    val jmap = new java.util.HashMap[String, String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/exact/path/to/metadata/location")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")
    im.setMetadataStoreParams(jmap)
  ```

  - For Python:
  ```python
    im = MetaIndexManager(spark, uri, 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend')
    md_backend_config = dict([
    ('spark.ibm.metaindex.parquet.mdlocation', "/exact/path/to/metadata/location"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")])
    im.setMetadataStoreParameters(md_backend_config)
  ```

  Note that when setting the type to `HIVE_TABLE_NAME` you should first set the parameter 'spark.ibm.metaindex.parquet.mdlocation'  in the table properties to point to the metadata location. If the location does not exist in the Hive table, the following fallback is used:

  `HIVE_TABLE_NAME` -> `HIVE_DB_NAME` (as base path) -> `/tmp/dataskipping_metadata` (as base path)

## Support for data skipping in Hive tables

The Parquet metadata store supports indexing and skipping data in Hive tables. Note that indexing is for Hive tables with partitions. To index tables without partitions, index the physical location directly.

- Indexing

  When indexing a Hive table and using a JVM configured base location, you should set the metadata type location to  `HIVE_TABLE_NAME` using:

  - For Scala:
  ```scala
    val im = new MetaIndexManager(spark, "myDb.myTable", ParquetMetadataBackend)

    // Configure the parameters to set this to hive table
    val jmap = new java.util.HashMap[String, String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "myDb.myTable")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "HIVE_TABLE_NAME")
    im.setMetadataStoreParams(jmap)
  ```

  - For Python:
  ```python
    md_backend = 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend'
    im = MetaIndexManager(spark, "myDb.myTable", md_backend)

    # Configure the MetaIndexManager specific parameters to set this to hive table
    md_backend_config = dict([('spark.ibm.metaindex.parquet.mdlocation', "myDb.myTable"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "HIVE_TABLE_NAME")])
    im.setMetadataStoreParameters(md_backend_config)
  ```
 This way, when indexing finishes, the metadata location parameter `spark.ibm.metaindex.parquet.mdlocation` is added to the table parameters automatically.

 Alternatively, you can save the metadata for the table in a specific location.
  - For Scala:
  ```scala
    val im = new MetaIndexManager(spark, "myDb.myTable", ParquetMetadataBackend)

    // Configure the parameters to set this to hive table
    val jmap = new java.util.HashMap[String, String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/location/to/metadata")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")
    im.setMetadataStoreParams(jmap)
  ```

  - For Python:
  ```python
    md_backend = 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend'
    im = MetaIndexManager(spark, "myDb.myTable", md_backend)

    # Configure the MetaIndexManager specific parameters to set this to hive table
    md_backend_config = dict([('spark.ibm.metaindex.parquet.mdlocation', "/location/to/metadata"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")])
    im.setMetadataStoreParameters(md_backend_config)
  ```

    And then update the location in the table properties manually:

    - For Scala and Python:
    ```
    spark.sql("ALTER TABLE myDb.myTable SET TBLPROPERTIES ('spark.ibm.metaindex.parquet.mdlocation'='/location/to/metadata')")
    ```

- Querying

  When querying a Hive table, the data skipping library first looks for the parameter  `spark.ibm.metaindex.parquet.mdlocation` in the table properties. If the parameter does not exist in the Hive table, the following fallback is used:

  `HIVE_TABLE_NAME` -> `HIVE_DB_NAME` (as base path) -> `/tmp/dataskipping_metadata` (as base path)

## Limitations

Data skipping might not work if type casting is used in the WHERE clause. For example, given a min/max index on a column with a `short` data type, the following query will not benefit from data skipping:

```SQL
select * from table where shortType > 1
```

Spark will evaluate this expression as `(cast(shortType#3 as int) > 1)` because the constant `1` is of type `Integer`.

Note that in some cases Spark can automatically cast the literal to the right type. For example, the previous query works for all other numerical types except for `byte` type, which would require casting as well.

To benefit from data skipping in such cases, make sure that the literal has the same type as the column type, for example:

```SQL
select * from table where shortType > cast(1 as short)
```

 ## Learn more
 {: #learn-more-data-skipping}

 Click the following link for code samples that show you how to use the configuration options described in this topic:

 - [Data skipping code samples](https://github.com/IBM-Cloud/IBM-Analytics-Engine/tree/master/data-skipping-samples)
