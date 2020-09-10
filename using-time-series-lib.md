---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-10"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Working with the time series library
{: #using-time-series-lib}

To get started working with the time series library, import the library to your Python notebook or application:

```python
# Import the package
import tspy
```

## Creating a time series
{: #creating-time-series}

To create a time series and use the library functions, you must decide on the data source. Supported data sources include:

- In-memory lists
- pandas DataFrames
- In-memory collections of observations (using the `ObservationCollection` construct)
- User-defined readers (using the `TimeSeriesReader` construct)

The following example shows ingesting data from an in-memory list:
```python
ts = tspy.time_series.list([5.0, 2.0, 4.0, 6.0, 6.0, 7.0])
ts
```

The output is as follows:
```
TimeStamp: 0     Value: 5.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 4.0
TimeStamp: 3     Value: 6.0
TimeStamp: 4     Value: 6.0
TimeStamp: 5     Value: 7.0
```

You can also operate on many time-series at the same time by using the `MultiTimeSeries` construct. A `MultiTimeSeries` is essentially a dictionary of time series, where each time series has its own unique key. The time series are not aligned in time.

The `MultiTimeSeries` construct provides similar methods for transforming and ingesting as the single time series construct:
```python
mts = tspy.multi_time_series.dict({
	"ts1": tspy.time_series.list([1.0, 2.0, 3.0]),
	"ts2": tspy.time_series.list([5.0, 2.0, 4.0, 5.0])
})
```

The output is the following:
```
ts2 time series
------------------------------
TimeStamp: 0     Value: 5.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 4.0
TimeStamp: 3     Value: 5.0
ts1 time series
------------------------------
TimeStamp: 0     Value: 1.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 3.0
```

## Interpreting time
{: #interpreting-time}

By default, a time series uses a `long` data type to denote when a given observation was created, which is referred to as a time tick. A time reference system is used for time series with timestamps that are human interpretable. See [Using time reference system](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-reference-system).

The following example shows how to create a simple time series where each index denotes a day after the start time of `1990-01-01`:
```python
import datetime
granularity = datetime.timedelta(days=1)
start_time = datetime.datetime(1990, 1, 1, 0, 0, 0, 0, tzinfo=datetime.timezone.utc)

ts = tspy.time_series.list([5.0, 2.0, 4.0, 6.0, 6.0, 7.0], granularity=granularity, start_time=start_time)
ts
```

The output is as follows:
```
TimeStamp: 1990-01-01T00:00Z     Value: 5.0
TimeStamp: 1990-01-02T00:00Z     Value: 2.0
TimeStamp: 1990-01-03T00:00Z     Value: 4.0
TimeStamp: 1990-01-04T00:00Z     Value: 6.0
TimeStamp: 1990-01-05T00:00Z     Value: 6.0
TimeStamp: 1990-01-06T00:00Z     Value: 7.0
```

## Performing simple transformations
{: #performing-transformations}

Transformations are functions which, when given one or more time series, return a new time series.

For example, to segment a time series into windows where each window is of `size=3`, sliding by 2 records, you can use the following method:
```python
window_ts = ts.segment(3, 2)
window_ts
```
The output is as follows:
```
TimeStamp: 0     Value: original bounds: (0,2) actual bounds: (0,2) observations: [(0,5.0),(1,2.0),(2,4.0)]
TimeStamp: 2     Value: original bounds: (2,4) actual bounds: (2,4) observations: [(2,4.0),(3,6.0),(4,6.0)]
```

This example shows adding 1 to each value in a time series:
```python
add_one_ts = ts.map(lambda x: x + 1)
add_one_ts
```

The output is as follows:
```
TimeStamp: 0     Value: 6.0
TimeStamp: 1     Value: 3.0
TimeStamp: 2     Value: 5.0
TimeStamp: 3     Value: 7.0
TimeStamp: 4     Value: 7.0
TimeStamp: 5     Value: 8.0
```
Or you can temporally left join a time series, for example `ts`  with another time series `ts2`:
```python
ts2 = tspy.time_series.list([1.0, 2.0, 3.0])
joined_ts = ts.left_join(ts2)
joined_ts
```

The output is as follows:
```
TimeStamp: 0     Value: [5.0, 1.0]
TimeStamp: 1     Value: [2.0, 2.0]
TimeStamp: 2     Value: [4.0, 3.0]
TimeStamp: 3     Value: [6.0, null]
TimeStamp: 4     Value: [6.0, null]
TimeStamp: 5     Value: [7.0, null]
```

### Using transformers
{: #using-transformers}

A rich suite of built-in transformers is provided in the transformers package. Import the package to use the provided transformer functions:

```python
from tspy.builders.functions import transformers
```

After you have added the package, you can transform data in a time series be using the `transform` method.

For example, to perform a difference on a time-series:
```python
ts_diff = ts.transform(transformers.difference())
```

Here the output is:
```
TimeStamp: 1     Value: -3.0
TimeStamp: 2     Value: 2.0
TimeStamp: 3     Value: 2.0
TimeStamp: 4     Value: 0.0
TimeStamp: 5     Value: 1.0
```

### Using reducers
{: #using-reducers}

Similar to the transformers package, you can reduce a time series by using methods provided by the reducers package. You can import the reducers package as follows:

```python
from tspy.builders.functions import reducers
```

After you have imported the package, use the `reduce` method to get the average over a time-series for example:
```python
avg = ts.reduce(reducers.average())
avg
```

This outputs:
```
5.0
```

Reducers have a special property that enables them to be used alongside segmentation transformations (hourly sum, avg in the window prior to an error occurring, and others). Because the output of a `segmentation + reducer` is a time series, the `transform` method is used.

For example, to segment into windows of size 3 and get the average across each window, use:
```python
avg_windows_ts = ts.segment(3).transform(reducers.average())
```

This results in:
```
imeStamp: 0     Value: 3.6666666666666665
TimeStamp: 1     Value: 4.0
TimeStamp: 2     Value: 5.333333333333333
TimeStamp: 3     Value: 6.333333333333333
```

## Graphing time series
{: #graphing-time-series}

Lazy evaluation is used when graphing a time series. When you graph a time series, you can do one of the following:

- Collect the observations of the time series, which returns an `ObservationCollection`
- Reduce the time series to a value or collection of values
- Perform save or print operations

For example, to collect and return all of the values of a timeseries:
```python
observations = ts.collect()
observations
```

This results in:
```
[(0,5.0),(1,2.0),(2,4.0),(3,6.0),(4,6.0),(5,7.0)]
```

To collect a range from a time series, use:
```python
observations = ts[1:3] # same as ts.get_values(1, 3)
observations
```
Here the output is:
```
[(1,2.0),(2,4.0),(3,6.0)]
```

Note that a time series is optimized for range queries if the time series is periodic in nature.

Using the `describe` on a current time series, also graphs the time series:
```python
describe_obj = ts.describe()
describe_obj
```

The output is:
```
min inter-arrival-time: 1
max inter-arrival-time: 1
mean inter-arrival-time: 1.0
top: 6.0
unique: 5
frequency: 2
first: TimeStamp: 0     Value: 5.0
last: TimeStamp: 5     Value: 7.0
count: 6
mean:5.0
std:1.632993161855452
min:2.0
max:7.0
25%:3.5
50%:5.5
75%:6.25
```

## Learn more

- [Time series key functionality](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series-key-functionality)
- [Time series functions](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-series-functions)
- [Time series lazy evaluation](/docs/AnalyticsEngine?topic=AnalyticsEngine-lazy-evaluation)
- [Using time reference system](/docs/AnalyticsEngine?topic=AnalyticsEngine-time-reference-system)
