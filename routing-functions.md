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

# Routing functions
{: #routing-functions}

The spatio-temporal library includes routing functions that list the edges that yield a path from one node to another node.

To show you how to use the routing functions in the spatio-temporal library, the code samples in this topic use the OSM map for the city of Atlanta downloaded from [OpenStreetMap](https://www.openstreetmap.org).

To calculate a route:
1. Create a router and read from the OSM map:
```
>>> router = stc.router()
>>> router.read_map('atlanta.osm')
Creating Road Network...
Road Network Created...
Creating Network Searcher...
Network Searcher Created...
```
2. Set the start and end points:
```
>>> start = stc.point(33.763261, -84.394897)
>>> end = stc.point(33.753900, -84.385121)
```
3. Find the best route with the minimal time cost (the fastest route timewise):
```
>>> best_time_route = router.compute_route(start, end, method='time')
# Check time cost, in the unit of hours
>>> best_time_route.cost
0,044188548756240675
# Check route path (only showing the first three points), which is a list of points in 3-tuple (osm_point_id, lat, lon)
>>> best_time_route.path[:3]
[(2036943312, 33.7631862, -84.3939405),
 (3523447568, 33.7632666, -84.3939315),
 (2036943524, 33.7633273, -84.3939155)]
```
4.Find the best route with minimal distance cost (the fastest route distance-wise):
```
>>> best_distance_route = router.compute_route(start, end, method='distance')
# Check distance cost, in the unit of meters
>>> best_distance_route.cost
2042,4082601271236
# Check route path (only showing the first three points), which is a list of points in 3-tuple (osm_point_id, lat, lon)
>>> best_distance_route.path[:3]
[(2036943312, 33.7631862, -84.3939405),
 (3523447568, 33.7632666, -84.3939315),
 (2036943524, 33.7633273, -84.3939155)]
```
