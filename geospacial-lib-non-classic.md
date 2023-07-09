---

copyright:
  years: 2017, 2020
lastupdated: "2023-02-03"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:external: target="_blank" .external}

# Working with the spatio-temporal library
{: #geospatial-geotemporal-lib-non-classic}

You can use the spatio-temporal library to expand your data science analysis in Python notebooks to include location analytics by gathering, manipulating and displaying imagery, GPS, satellite photography and historical data.

You can use the spatio-temporal lib for applications that run in a standalone {{site.data.keyword.iae_full_notm}} cluster, which you create for your data analysis processing, or in solutions that use {{site.data.keyword.iae_full_notm}}, for example in the Spark environments available in {{site.data.keyword.DSX_full}}.

## Key functions
{: #geospatial-geotemporal-lib-non-classic-1}

The geospatial library includes functions to read and write data, topological functions, geohashing, indexing, ellipsoidal and routing functions.

Key aspects of the library include:
- All calculated geometries are accurate without the need for projections.
- The geospatial functions, when run in {{site.data.keyword.iae_full_notm}} standalone or in a solution that uses {{site.data.keyword.iae_full_notm}} take advantage of the distributed processing capabilities provided by Spark.
- The library includes native geohashing support for geometries used in simple aggregations and in {{site.data.keyword.cos_full_notm}}  indexing, whereby improving storage retrieval considerably.
- The library supports extensions of Spark distributed joins.
- The library supports the SQL/MM extensions to Spark SQL.

## Getting started with the library
{: #geospatial-geotemporal-lib-non-classic-2}

Before you can start using the library in a notebook, you must register `STContext` in your notebook to access the `st` functions.

To register `STContext`:
```python
from pyst import STContext
stc = STContext(spark.sparkContext._gateway)
```

## Next steps
{: #geospatio-next-steps}

After you have registered `STContext` in your notebook, you can begin exploring the spatio-temporal library for:

- [Functions to read and write data](/docs/AnalyticsEngine?topic=AnalyticsEngine-read-write-data)
- [Topological functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-topological-functions)
- [Geohashing functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-geohashing-functions)
- [Geospatial indexing functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-spatial-indexing-functions)
- [Ellipsoidal functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-ellipsoidal-metrics)
- [Routing functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-routing-functions)

## Learn more
{: #learn-more-goespatio-lib}

Check out the following Python notebooks to learn how to use the spacio-temporal library functions in Python notebooks. To access these notebooks, you need an {{site.data.keyword.DSX_full}} instance. See [Provisioning an {{site.data.keyword.DSX_full}} instance](https://cloud.ibm.com/catalog/services/watson-studio){: external}.

- [Use the spatio-temporal library for location analytics](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/92c6ab6ea922d1da6a2cc9496a277005){: external}
- [Use spatial indexing to query spatial data](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/a7432f0c29c5bda2fb42749f3628d981){: external}
- [Spatial Queries in PySpark](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/27ecffa80bd3a386fffca1d8d1256ba7){: external}
