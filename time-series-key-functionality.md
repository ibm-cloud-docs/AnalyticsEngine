---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-27"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Time series key functionality
{: #time-series-key-functionality}

The time series library provides various functions on univariate, multivariate, multi-key time series as well as numeric and categorical types. The functionality provided by the library can be broadly categorized into:

- Time series I/O, for creating and saving time series data
- Time series functions, transforms, windowing or segmentation, and reducers
- Time series SQL and SQL extensions to Spark to enable executing scalable time series functions

Some of the key functionality is shown in the following sections using examples.

## Time series I/O
{: #time-series-io}

The primary input and output (I/O) functionality for a time series is through a pandas DataFrame or a Python list. The following code sample shows  constructing a time series from a DataFrame:
```Python
>>> import numpy as np
>>> import pandas as pd
>>> data = np.array([['', 'key', 'timestamp', "value"],['', "a", 1, 27], ['', "b", 3, 4], ['', "a", 5, 17], ['', "a", 3, 7], ['', "b", 2, 45]])
>>> df = pd.DataFrame(data=data[1:, 1:], index=data[1:, 0], columns=data[0, 1:]).astype(dtype={'key': 'object', 'timestamp': 'int64', 'value': 'float64'})
>>> df
key  timestamp  value
  a          1   27.0
  b          3    4.0
  a          5   17.0
  a          3    7.0
  b          2   45.0

#Create a timeseries from a dataframe, providing a timestamp and a value column
>>> ts = tspy.time_series.df(df, "timestamp", "value")
>>> ts
TimeStamp: 1     Value: 27.0
TimeStamp: 2     Value: 45.0
TimeStamp: 3     Value: 4.0
TimeStamp: 3     Value: 7.0
TimeStamp: 5     Value: 17.0
```

To revert from a time series back to a pandas DataFrame, use the `to_df` function:
```python
>>> import tspy
>>> ts_orig = tspy.time_series.list([1.0, 2.0, 3.0])
>>> ts_orig
TimeStamp: 0     Value: 1
TimeStamp: 1     Value: 2
TimeStamp: 2     Value: 3

>>> df = ts_orig.to_df()
>>> df
   timestamp  value
0          0      1
1          1      2
2          2      3
```

## Data model
{: #data-model}

Time series data does not have any standards for the model and data types, unlike some data types such as spatial, which are governed by a standard such as Open Geospatial Consortium (OGC). The challenge with time series data is the wide variety of functions that need to be supported, similar to that of Spark Resilient Distributed Datasets (RDD).

The data model allows for a wide variety of operations ranging across different forms of segmentation or windowing of time series, transformations or conversions of one time series to another, reducers that compute a static value from a time series, joins that join multiple time series, and collectors of time series from different time zones. The time series library enables the plug-and-play of new functions while keeping the core data structure unchangeable. The library also support numeric and categorical typed timeseries.

With time zones and various human readable time formats, a key aspect of the data model is support for Time Reference System (TRS). Every time series is associated with a TRS (system default), which can be remapped to any specific choice of the user at any time, enabling easy transformation of a specific time series or a segment of a time series. See [Using time reference system](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-reference-system).

Further, with the need for handling large scale time series, the  library offers a lazy evaluation construct by providing a mechanism for identifying the maximal narrow temporal dependency. This construct is very similar to that of a Spark computation graph, which also loads data into memory on as needed basis and realizes the computations only when needed.

## Time series data types
{: #time-series-data-types}

You can use multiple data types as an element of a time series, spanning numeric, categorical, array, and dictionary data structures.

The following data types are supported in a time series:

| Data type     | Description          |
|---------------|----------------------|
| numeric       | Time series with univariate observations of numeric type including double and integer. For example:`[(1, 7.2), (3, 4.5), (5, 4.5), (5, 4.6), (5, 7.1), (7, 3.9), (9, 1.1)]`|
| numeric array | Time series with multivariate observations of numeric type, including double array and integer array. For example: `[(1, [7.2, 8.74]), (3, [4.5, 9.44]), (5, [4.5, 10.12]), (5, [4.6, 12.91]), (5, [7.1, 9.90]), (7, [3.9, 3.76])]`|
| string        |	Time series with univariate observations of type string, for example: `[(1, "a"), (3, "b"), (5, "c"), (5, "d"), (5, "e"), (7, "f"), (9, "g")]`|
| string array  |	Time series with multivariate observations of type string array, for example: `[(1, ["a", "xq"]), (3, ["b", "zr"]), (5, ["c", "ms"]), (5, ["d", "rt"]), (5, ["e", "wu"]), (7, ["f", "vv"]), (9, ["g", "zw"])]`|
| segment       | Time series of segments. The output of the `segmentBy` function, can be any type, including numeric, string, numeric array, and string array. For example: `[(1,[(1, 7.2), (3, 4.5)]), (5,[(5, 4.5), (5, 4.6), (5, 7.1)]), (7,[(7, 3.9), (9, 1.1)])]`|
| dictionary    |	Time series of dictionaries. A dictionary can have arbitrary types inside it |

## Time series functions

You can use different functions in the provided time series packages to analyze time series data to extract meaningful information with which to create models that can be used to predict new values based on previously observed values. See [Time series functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series-functions).
