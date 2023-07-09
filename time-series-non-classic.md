---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-07"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Time series library
{: #time-series}

A time series is a sequence of data values measured at successive, though not necessarily regular, points in time. The time series library allows you to perform various key operations on time series data, including segmentation, forecasting, joins, transforms, and reducers.

The library support various time series types, including numeric, categorical, and arrays. Examples of time series data include:

- Stock share prices and trading volumes
- Clickstream data
- Electrocardiogram (ECG) data
- Temperature or seismographic data
- Network performance measurements
- Network logs
- Electricity usage as recorded by a smart meter and reported via an Internet of Things data feed

An entry in a time series is called an observation. Each observation comprises a time tick, a 64-bit integer that indicates when the observation was made, and the data that was recorded for that observation. The recorded data can be either numerical, for example, a temperature or a stock share price, or categorical, for example, a geographic area. A time series can but must not necessarily be associated with a time reference system (TRS), which defines the granularity of each time tick and the start time.

The time series library is Python only.

## Next step
{: #time-series-1}

- [Working with the time series library](/docs/AnalyticsEngine?topic=AnalyticsEngine-using-time-series-lib)

## Learn more
{: #time-series-2}

- [Time series key functionality](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series-key-functionality)
