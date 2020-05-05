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

# Data skipping for Spark SQL
{: #data-skipping}

Data skipping can significantly boost the performance of SQL queries by skipping over irrelevant data objects or files based on a summary metadata associated with each object.

For every column in the object, the summary metadata might include minimum and maximum values, a list or bloom filter of the appearing values, or other metadata which succinctly represents the data in that column. This metadata is used during query evaluation to skip over objects which have no relevant data.

All Spark native data formats are supported, including Parquet, ORC, CSV, JSON and Avro. Data skipping is a performance optimization feature which means that using data skipping does not affect the content of the query results.

To use this feature, you need to create indexes on one or more columns of the data set. After this is done, Spark SQL queries can benefit from data skipping. In general, you should index the columns which are queried most often in the WHERE clause.

Three index types are supported:

| Index type  | Description  | Applicable to predicates in WHERE clauses  | Column types |
|------------|--------------|--------------|--------------|
| MinMax |Stores minimum and maximum values for a column | <,<=,=,>=,> | All types except for complex types. See [Supported Spark SQL data types](https://spark.apache.org/docs/latest/sql-reference.html#data-types). |
| ValueList | Stores the list of unique values for the column | =,IN,LIKE | All types except for complex types. See [Supported Spark SQL data types](https://spark.apache.org/docs/latest/sql-reference.html#data-types).|
| BloomFilter | Uses a bloom filter to test for set membership | =,IN | Byte, String, Long, Integer, Short |

You should use bloom filters for columns with very high cardinality. Index creation invokes a Spark job which writes metadata (indexes) to a user specified location, in Parquet format.

Note that the metadata is typically much smaller than the data. If changes are made to some of the objects in the data set after the index was created, data skipping will still work correctly but will not skip the changed objects. To avoid this, you should refresh the indexes.

## Geospatial data skipping

 Data skipping is also supported for queries on geospatial datasets with latitude and longitude columns using [geospatial functions](https://www.ibm.com/support/knowledgecenter/en/SSCJDQ/com.ibm.swg.im.dashdb.analytics.doc/doc/geo_functions.html) from the [spatio-temporal library](/docs/AnalyticsEngine?topic=AnalyticsEngine-geospacial-lib).

 In order to benefit from data skipping you can collect min/max indexes on the latitude and longitude columns.

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