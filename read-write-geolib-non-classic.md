---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-13"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Functions to read and write data
{: #read-write-data}

`pyst` supports most of the common geospatial formats, including  geoJSON and WKT.

## GeoJSON format

To work with files in geoJSON format in a Python notebook, first create a geoJSON reader and writer:
```
geojson_reader = stc.geojson_reader()
geojson_writer = stc.geojson_writer()
```
The following code snippets show reading live location data in geoJSON format available in Mapbox:

- Read the contents of a GeoJSON file copied to your local machine from Mapbox and display the first three lines of the file:
    ```
    ! wget -q https://www.mapbox.com/help/data/stations.geojson
    df = geojson_reader.read('stations.geojson')
    df.head(3)
    ```
- Read content in GeoJSON format from a Python dictionary:
    ```
    data = json.load(open('stations.geojson'))
    df = geojson_reader.read(data)
    df.head(3)
    ```
- Read data directly from Mapbox by using a URL and display the first three lines of the file:
    ```
    df = geojson_reader.read(‘https://www.mapbox.com/help/data/stations.geojson’) df.head(3)
    ```
- Read binary data (useful when reading streaming bytes from {{site.data.keyword.cos_full_notm}} for example):
    ```
    # Assume station.geojson is in IBM Cloud Object Storage in the bucket "test"
    client = ibm_boto3.client('s3')
    streaming_body_2 = client.get_object(Bucket='test', Key='station.geojson')['Body']
    df = shapefile_reader.read(streaming_body_2.read())
    df.head(3)
    ```
- Write the contents in a pandas DataFrame to a Python dictionary:
    ```
    geojson_writer.write(df)
    ```
- Write the contents of a pandas DataFrame to a GeoJSON file:
    ```
    geojson_writer.write(df, file_name='stations_out.geojson')
    ```

## WKT strings

To work with WKT strings in a Python notebook, first create a WKT reader and writer:
```
wkt_reader = stc.wkt_reader()
wkt_writer = stc.wkt_writer()
```

The following code snippets show reading from and writing to a WKT string:

- Read a WKT string to a geometry object:
    ```
    >>> westchester_WKT = 'POLYGON((-73.984 41.325,-73.948 41.33,-73.78 41.346,-73.625 41.363,-73.545 41.37,-73.541 41.368,-73.547 41.297,-73.485 41.223,-73.479 41.215,-73.479 41.211,-73.493 41.203,-73.509 41.197,-73.623 41.144,-73.628 41.143,-73.632 41.14,-73.722 41.099,-73.714 41.091,-73.701 41.073,-73.68 41.049,-73.68 41.047,-73.673 41.041,-73.672 41.038,-73.668 41.035,-73.652 41.015,-73.651 41.011,-73.656 41,-73.655 40.998,-73.656 40.995,-73.654 40.994,-73.654 40.987,-73.617 40.952,-73.618 40.946,-73.746 40.868,-73.751 40.868,-73.821 40.887,-73.826 40.886,-73.84 40.89,-73.844 40.896,-73.844 40.9,-73.85 40.903,-73.853 40.903,-73.854 40.9,-73.859 40.896,-73.909 40.911,-73.92 40.912,-73.923 40.914,-73.923 40.918,-73.901 40.979,-73.894 41.023,-73.893 41.043,-73.896 41.071,-73.894 41.137,-73.94 41.207,-73.965 41.24,-73.973 41.244,-73.975 41.247,-73.976 41.257,-73.973 41.266,-73.95 41.288,-73.966 41.296,-73.98 41.309,-73.984 41.311,-73.987 41.315,-73.987 41.322,-73.984 41.325))'
    >>> westchester = wkt_reader.read(westchester_WKT)
    >>> westchester.area()
    1363618331.5760047
    >>> westchester.centroid()
    Point(41.15551622739597, -73.7592843233704)
    >>> westchester.get_bounding_box()
    BoundingBox: Lower Corner: Point(40.86800000000001, -73.987), Upper Corner: Point(41.36999999999998, -73.479)
    ```
- Write a geometry object to a WKT string:
    ```
    >>> wkt_writer.write(westchester)
    'POLYGON ((-73.984 41.325, -73.987 41.322, -73.987 41.315, -73.984 41.311, -73.98 41.309, -73.966 41.296, -73.95 41.288, -73.973 41.266, -73.976 41.257, -73.975 41.247, -73.973 41.244, -73.965 41.24, -73.94 41.207, -73.894 41.137, -73.896 41.071, -73.893 41.043, -73.894 41.023, -73.901 40.979, -73.923 40.918, -73.923 40.914, -73.92 40.912, -73.909 40.911, -73.859 40.896, -73.854 40.9, -73.853 40.903, -73.85 40.903, -73.844 40.9, -73.844 40.896, -73.84 40.89, -73.826 40.886, -73.821 40.887, -73.751 40.868, -73.746 40.868, -73.618 40.946, -73.617 40.952, -73.654 40.987, -73.654 40.994, -73.656 40.995, -73.655 40.998, -73.656 41.0, -73.651 41.011, -73.652 41.015, -73.668 41.035, -73.672 41.038, -73.673 41.041, -73.68 41.047, -73.68 41.049, -73.701 41.073, -73.714 41.091, -73.722 41.099, -73.632 41.14, -73.628 41.143, -73.623 41.144, -73.509 41.197, -73.493 41.203, -73.479 41.211, -73.479 41.215, -73.485 41.223, -73.547 41.297, -73.541 41.368, -73.545 41.37, -73.625 41.363, -73.78 41.346, -73.948 41.33, -73.984 41.325))'
    ```

## Direct Input

Besides reading geometry objects from different sources, direct input is also supported and each geometry type comes with a constructor:

- Point: `point(lat, lon)`
- LineSegment: `line_segment(start_point, end_point)`
-LineString: `line_string([point_1, point_2, …])` or `line_string([line_segment_1, line_segment_2, …])`
- Ring: `ring([point_1, point_2, …]) or ring([line_segment_1, line_segment_2, …])`
- Polygon: `polygon(exterior_ring, [interior_ring_1, interior_ring_2, …])`
- MultiGeometry: `multi_geometry(geom_1, geom_2, …)`
- MultiPoint: `multi_point(point_1, point_2, …)`
- MultiLineString: `multi_line_string(line_string_1, line_string_2, …)`
- MultiPolygon: `multi_polygon(polygon_1, polygon_2, …)`
- Null Geometry: `null_geometry()`
- FullEarth: `full_earth()`
- BoundingBox: `bounding_box(lower_lat, lower_lon, upper_lat, upper_lon)`

Some examples:
```
point_1 = stc.point(37.3, -74.4)
point_2 = stc.point(37.5,-74.1)
point_3 = stc.point(38.1, -74.4)
line_segment = stc.line_segment(point_1, point_2)
line_string = stc.line_string([point_1, point_2, point_3])
ring = stc.ring([point_1, point_2, point_3, point_1])
poly = stc.polygon(ring)
multi_points = stc.multi_point([point_1, point_2, point_3])

>>> poly.area()
1179758985.44891
>>> multi_points
MultiPoint(Point(37.5, -74.1), Point(38.1, -74.4), Point(37.3, -74.4))
>>> next(multi_points)
Point(37.5, -74.1)
>>> next(multi_points)
Point(38.1, -74.4)
>>> next(multi_points)
Point(37.3, -74.4)
```
