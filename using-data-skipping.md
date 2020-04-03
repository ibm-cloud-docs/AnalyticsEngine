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

# Working with the data skipping libraries
{: #using-data-skipping}

Data skipping indexes apply to Parquet, ORC, CSV, JSON and Avro data sets and store metadata for each object in the data set. When a SQL query is triggered, this metadata is scanned and all objects whose metadata does not overlap with the given predicate in the query are skipped.

## Using the data skipping libraries

The following steps show you how to inject a skipping rule and enable data skipping. You are shown which libraries to import, how to configure where to save the metadata that is created for each data object, how to index the data set or refresh an existing index, and finally how to query the data set and then view the statistics of the data that was skipped. The code snippets are for Python and Scala.

1. Create a Spark session and inject the data skipping rule:

  - For Scala:
  ```Scala
    import com.ibm.metaindex.metadata.metadatastore.parquet.{Parquet,ParquetMetadataBackend}
    import com.ibm.metaindex.{MetaIndexManager,Registration}
    import org.apache.spark.sql.SparkSession
    val spark=SparkSession
         .builder()
         .appName("Data Skipping Library usage example")
         // if hive support is needed
         // .enableHiveSupport()
         .getOrCreate()
    // inject the the data skipping rule
    MetaIndexManager.injectDataSkippingRule(spark)

    // enable data skipping
    MetaIndexManager.enableFiltering(spark)
   ```
   - For Python:
   ```python
   from pyspark.sql import SparkSession
   from metaindex import MetaIndexManager
   spark=(SparkSession.builder
         .appName("Data Skipping Library usage example")
         # if hive support is needed
         # .enableHiveSupport()
         .getOrCreate()
   )

    # inject the data skipping rule
    MetaIndexManager.injectDataSkippingRule(spark)

    # enable data skipping
    MetaIndexManager.enableFiltering(spark)
   ```
1. Configure the MetaIndexManager and Metadatastore parameters. In this example, you will set the JVM wide parameter to a base path to store all of the indexes. Metadata can be stored on the same storage system as the data however, not under the same path. For more configuration options, see [Data skipping configuration options](/docs/AnalyticsEngine?topic=AnalyticsEngine-data-skipping-config-options).

   - For Scala:
   ```scala
    // Set the default Metadatastore
    Registration.setDefaultMetaDataStore(Parquet)

    // Set the JVM wide metadatalocation
    val jmap = new java.util.HashMap[String,String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/path/to/base/metadata/location")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")
    MetaIndexManager.setConf(jmap)

    // (Optional) You can set the metadatalocation parameters specifically for a
    // certain data set by creating a MetaIndexManager instance with the needed parameters
    // uri is the datapath.
    // to set a specific location for a hive table please set in the table properties the parameter
    // 'spark.ibm.metaindex.parquet.mdlocation' to point to the metadata location

    // val im = new MetaIndexManager(spark, uri, ParquetMetadataBackend)
    // val jmap = new java.util.HashMap[String, String]()
    // jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/exact/path/to/metadata/location")
    // jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")
    // im.setMetadataStoreParams(jmap)
    ```
   - For Python:
    ```python
    # Set the default Metadatastore
    MetaIndexManager.setDefaultMetaDataStore(spark, 'com.ibm.metaindex.metadata.metadatastore.parquet.Parquet')

    # Set the JVM wide metadata location
    md_backend_config = dict([
    ('spark.ibm.metaindex.parquet.mdlocation', "/path/to/base/metadata/location"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_BASE_PATH_LOCATION")])
    MetaIndexManager.setConf(spark, md_backend_config)

    # (Optional) You can set the metadata location parameters specifically for a
    # certain dataset by creating a MetaIndexManager instance with the needed parameters
    # uri is the datapath.
    # to set a specific location for a hive table please set in the table properties the parameter
    # 'spark.ibm.metaindex.parquet.mdlocation' to point to the metadata location

    # im = MetaIndexManager(spark, uri, 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend')
    # md_backend_config = dict([
    # ('spark.ibm.metaindex.parquet.mdlocation', "/exact/path/to/metadata/location"),
    # ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")])
    # im.setMetadataStoreParameters(md_backend_config)
  ```
1. Index the data set or refresh an existing index. Skip this step if the data set is already indexed.

   - For Scala:
   ```scala
    val datasetLocation = "/path/to/data/"
    // Create MetaIndexManager instance with Parquet Metadatastore backend
    val im = new MetaIndexManager(spark, datasetLocation, ParquetMetadataBackend)

    // (Optional) You can set the metadata location parameters specifically for a
    // certain dataset by creating a MetaIndexManager instance with the needed parameters
    // to set a specific location for a hive table please set in the table properties the parameter
    // 'spark.ibm.metaindex.parquet.mdlocation' to point to the metadata location

    // val jmap = new java.util.HashMap[String, String]()
    // jmap.put("spark.ibm.metaindex.parquet.mdlocation", "/exact/path/to/metadata/location")
    // jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")
    // im.setMetadataStoreParams(jmap)

    // If the index already exists and you would like to refersh it use:
    // (Note that after creating the index for the first time you should use refresh to update the index)
    val reader = spark.read.format("parquet")
    im.refreshIndex(reader).show(false)

    // If the index does not exist or you would like to remove it and reindex use:
    if (im.isIndexed()) {
     im.removeIndex()
    }

    // Example of building a min/max index on a column named `temp`
    im.indexBuilder().addMinMaxIndex("temp").build(reader).show(false)

    // View index status
    im.getIndexStats(reader).show(false)
   ```

   - For Python:
   ```python
    datasetLocation = "/path/to/data/"
    # Create MetaIndexManager instance with Parquet Metadatastore backend
    md_backend = 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend'
    im = MetaIndexManager(spark, datasetLocation, md_backend)

    # (Optional) You can set the metadata location parameters specifically for a
    # certain dataset by creating a MetaIndexManager instance with the needed parameters
    # to set a specific location for a hive table please set in the table properties the parameter
    # 'spark.ibm.metaindex.parquet.mdlocation' to point to the metadata location

    # md_backend_config = dict([
    #('spark.ibm.metaindex.parquet.mdlocation', "/exact/path/to/metadata/location"),
    # ("spark.ibm.metaindex.parquet.mdlocation.type", "EXPLICIT_LOCATION")])
    # im.setMetadataStoreParameters(md_backend_config)

    # If the index already exists and you would like to refersh it use:
    # (Note that after creating the index for the first time you should use refresh to update the index)
    reader = spark.read.format("parquet").option("header","true")    
    im.refreshIndex(reader).show(10, False)

    # If the index does not exist or you would like to remove it and reindex use:
    if im.isIndexed():
	     im.removeIndex()

    # Example of building a min/max index on a column named `temp`
    im.indexBuilder().addMinMaxIndex("temp").build(reader).show(10, False)

    # View index status
    im.indexStats(reader).show(10, False)
   ```

    Note that each of the index types has a corresponding method in the indexBuilder class of the form:

    `add[IndexType]Index(<index_params>)`

    For example:
    - `addMinMaxIndex(col: String)`
    - `addValueListIndex(col: String)`
    - `addBloomFilterIndex(col: String)`

   - Indexing Hive tables.

   Note that indexing is for hive tables with partitions. To index tables without partitions index the physical location directly.

     For Scala:
    ```scala
    // Create MetaIndexManager instance with Parquet Metadatastore backend
    // the second parameter below is the table identifier in the form of <dbname>.<tablename>
    val im = new MetaIndexManager(spark, "myDb.myTable", ParquetMetadataBackend)

    // Configure the parameters to set this to hive table
    val jmap = new java.util.HashMap[String, String]()
    jmap.put("spark.ibm.metaindex.parquet.mdlocation", "myDb.myTable")
    jmap.put("spark.ibm.metaindex.parquet.mdlocation.type", "HIVE_TABLE_NAME")
    im.setMetadataStoreParams(jmap)

    // If the index already exists and you would like to refersh it use:
    // (Note that after creating the index for the first time you should use refresh to update the index)
    im.refreshIndex().show(false)

    // If the index does not exist or you would like to remove it and reindex use:
    if (im.isIndexed()) {
      im.removeIndex()
    }

    // Example of building a min/max index on a column named `temp`
    im.indexBuilder().addMinMaxIndex("temp").build().show(false)

    // View index status
    im.getIndexStats().show(false)
   ```

    For Python:
   ```python
    # Create MetaIndexManager instance with Parquet Metadatastore backend
    md_backend = 'com.ibm.metaindex.metadata.metadatastore.parquet.ParquetMetadataBackend'
    im = MetaIndexManager(spark, "myDb.myTable", md_backend)

    # Configure the MetaIndexManager specific parameters to set this to hive table
    md_backend_config = dict([('spark.ibm.metaindex.parquet.mdlocation', "myDb.myTable"),
    ("spark.ibm.metaindex.parquet.mdlocation.type", "HIVE_TABLE_NAME")])
    im.setMetadataStoreParameters(md_backend_config)

    # If the index already exists and you would like to refersh it use:
    # (Note that after creating the index for the first time you should use refresh to update the index)
    im.refreshIndex().show(10, False)

    # If the index does not exist or you would like to remove it and reindex use:
    if im.isIndexed():
	     im.removeIndex()

    # Example of building a min/max index on a column named `temp`
    im.indexBuilder().addMinMaxIndex("temp").build().show(10, False)

    # View index status
    im.indexStats().show(10, False)
   ```
1. Now you can continue querying the data set. In the following sample code snippets, `weather` is either a hive table or a view on a data frame that was read.

   - For Scala:
   ```scala
     val query = "SELECT * FROM weather WHERE temp > 30"
     val queryDF = spark.sql(query)
     queryDF.show()

     // View skipping stats of last query
     MetaIndexManager.getLatestQueryAggregatedStats(spark).show(false)

     // (Optional) clear the stats for the next query (otherwise, stats will acummulate)
     MetaIndexManager.clearStats()
   ```
   - Python:
   ```python
     query = "SELECT * FROM weather WHERE temp > 30"
     queryDF = spark.sql(query)
     queryDF.show()

     # View skipping stats of last query
     MetaIndexManager.getLatestQueryAggregatedStats(spark).show(10, False)

     # (Optional) clear the stats for the next query (otherwise, stats will acummulate)
     MetaIndexManager.clearStats(spark)
   ```
