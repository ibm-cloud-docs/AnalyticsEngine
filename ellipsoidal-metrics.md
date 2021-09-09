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

# Using ellipsoidal metrics
{: #ellipsoidal-metrics}

You can use ellipsoidal metrics to calculate the distance between points.

Examples of metrics you can compute:

- Compute the radians between two points using `azimuth`:
    ```
    >>> p1 = stc.point(47.1, -73.5)
    >>> p2 = stc.point(47.6, -72.9)
    >>> stc.eg_metric.azimuth(p1, p2)
    0,6802979449118038
    ```
- Compute the distance between two points in the units of the underlying data (typically in meters):
    ```
    >>> eg_metric.distance(p1, p2)
    71730,66213673435
    ```
- Compute the landing point given a starting point, a heading (in radians), and the distance in the units of the underlying data (typically in meters):
    ```
    >>> point = p1
    >>> heading = eg_metric.azimuth(p1, p2)
    >>> distance = eg_metric.distance(p1, p2)
    >>> eg_metric.destination_point(p1, heading, distance)
    Point(47.60000000001233, -72.89999999998498)
    ```
