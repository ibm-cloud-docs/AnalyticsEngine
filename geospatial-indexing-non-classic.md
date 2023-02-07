---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-08"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Geospatial indexing functions
{: #spatial-indexing-functions}

With the spatio-temporal library, you can use functions to index points within a region, on a region containing points, and points within a radius to enable fast queries on this data during location analysis.

Perform the following steps to create and use a spatial index. The example shows how to leverage a spatial index to find US counties for given queries on certain locations.

1. Get the county boundaries. For this, use a `geojson_reader` to read a GeoJSON file, which contains the US county boundaries, to a pandas DataFrame and display the first three lines of content:
    ```
    county_df = stc.geojson_reader().read('http://eric.clst.org/assets/wiki/uploads/Stuff/gz_2010_us_050_00_20m.json')
    county_df.head(3)
    ```
2. Create county spatial index. `pyst` provides several spatial indexing algorithms, including `grid_index`, `r_star_tree_index` and `tessellation_index`. The example shows how to create a tessellation index.

    To create a tessellation spatial index, you need to set two parameters:

 - **Bounding box**: Defines the boundary of the spatial index. If you know exactly where your geometries are and are able to define a boundary that contains all these geometries, you should pass this bounding box information to the function because it will reduce the amount of *tiles* that need to be created and increase performance. However, if you don’t know the geometries or you want to play safe to not exclude any geometries that might potentially fall outside the given bounding box (both are very common situations), you can use the whole earth as the bounding box by simply leaving the `bbox` parameter setting at `None`, which is the default value.

 - **Tile size**: Defines the size of a tile in a tessellation index. The value is given by the length of the tile in the unit of meters. You should provide a tile size that is close to the size of your geometries for better performance. For example, if your geometries are 100 km2 (i.e. 10^8 m2), then 10^4 m could be a good value for tile size.

    With a value for bounding box and tile size, you can now create the spatial index and import your geometries into the spatial index. Use the `from_df` function to move the geometries from a pandas DataFrame to a spatial index. For this, you only need to specify the name of the geometry ID column and the name of the geometry column. Set the third parameter, `verbosity`, which controls log processing, to `error` so that only summary and failure logs are displayed.

    ```
    >>> tile_size = 100000
    >>> si = stc.tessellation_index(tile_size=tile_size) # we leave bbox as None to use full earth as boundingbox
    >>> si.from_df(county_df, 'NAME', 'geometry', verbosity='error')
    3221 entries processed, 3221 entries successfully added
    ```
3. Perform spatial index queries. For quering a spatial index, `pyst` provides the following different query APIs: `contained_in`, `contained_in_with_info`, `containing`, `containing_with_info`, `intersects`, `intersects_with_info`, `within_distance`, `within_distance_with_info`, `nearest_neighbors`, `nearest_neighbors_with_info`.

    Sample queries:

    - Which county does White Plains Hospital belongs to?  Which county polygon contains the point location of White Plains Hospital?
        ```
        >>> white_plains_hospital = stc.point(41.026132, -73.769585)
        >>> si.containing(white_plains_hospital)
        ['Westchester']
        ```
    - Which county is the city of White Plains located in? Which county polygon contains the polygon of White Plains?
        ```
        >>> white_plains_WKT = 'POLYGON((-73.792 41.024,-73.794 41.031,-73.779 41.046,-73.78 41.049,-73.779 41.052,-73.776 41.054,-73.775 41.057,-73.767 41.058,-73.769 41.062,-73.768 41.067,-73.762 41.073,-73.759 41.074,-73.748 41.069,-73.746 41.056,-73.742 41.056,-73.74 41.053,-73.74 41.049,-73.749 41.04,-73.748 41.035,-73.739 41.034,-73.729 41.029,-73.725 41.025,-73.72 41.016,-73.717 41.015,-73.716 41.006,-73.718 41.002,-73.732 40.988,-73.732 40.985,-73.739 40.979,-73.745 40.978,-73.749 40.981,-73.749 40.986,-73.751 40.986,-73.756 40.991,-73.759 40.991,-73.76 40.993,-73.765 40.994,-73.769 40.997,-73.774 41.002,-73.775 41.006,-73.788 41.018,-73.792 41.024))'
        >>> wkt_reader = stc.wkt_reader()
        >>> white_plains = wkt_reader.read(white_plains_WKT)
        >>> si.containing(white_plains)
        ['Westchester']
        ```
    - Which county is the city of White Plains located in? The result is a list of tuples with value, geometry, and distance.
        ```
        >>> si.containing_with_info(white_plains)
        [('Westchester',
          MultiPolygon(Polygon: Boundary: Ring(LineSegment(Point(40.886299, -73.767176), Point(40.886899, -73.767276)), LineSegment(Point(40.886899, -73.767276), Point(40.887599, -73.768276)), LineSegment(Point(40.887599, -73.768276), Point(40.888399, -73.770576)), ...) Interiors: , Polygon: Boundary: Ring(LineSegment(Point(41.198434, -73.514617), Point(41.200814, -73.509487)), LineSegment(Point(41.200814, -73.509487), Point(41.21276, -73.482709)), LineSegment(Point(41.21276, -73.482709), Point(41.295422, -73.550961)), ...) Interiors: ),
          0.0)]
        ```
    - Which are the 3 nearest counties to White Plains Hospital. The result includes their distances.
        ```
        >>> counties = si.nearest_neighbors_with_info(white_plains_hospital, 3)
        >>> for county in counties:
        ...     print(county[0], county[2])
        Westchester 0.0
        Fairfield 7320.602641166855
        Rockland 10132.182241119823
        ```
    - Which are the counties within 20 km of White Plains Hospital? The result is sorted by their distances.
        ```
        >>> counties = si.within_distance_with_info(white_plains_hospital, 20000)
        >>> counties.sort(key=lambda tup: tup[2])
        >>> for county in counties:
        ...     print(county[0], county[2])
        Westchester 0.0
        Fairfield 7320.602641166855
        Rockland 10132.182241119823
        Bergen 10934.1691335908
        Bronx 15683.400292349625
        Nassau 17994.425235412604
        ```
