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

# Time series functions
{: #time-series-functions}

Time series functions are aggregate functions that operate on  sequences of data values measured at points in time.

The following sections describe some of the time series functions available in different time series packages.  

## Transforms
{: #time-series-transforms}

Transforms are functions that are applied on a time series resulting in another time series. The time series library supports various types of transforms, including provided transforms (by using `from tspy.functions import transformers`) as well as user defined transforms.

The following sample shows some provided transforms:
```python
#Interpolation
>>> ts = tspy.time_series([1.0, 2.0, 3.0, 4.0, 5.0, 6.0])
>>> periodicity = 2
>>> interp = interpolators.nearest(0.0)
>>> interp_ts = ts.resample(periodicity, interp)
>>> interp_ts.print()
TimeStamp: 0     Value: 1.0
TimeStamp: 2     Value: 3.0
TimeStamp: 4     Value: 5.0

#Fillna
>>> shift_ts = ts.shift(2)
    print("shifted ts to add nulls")
    print(shift_ts)
    print("\nfilled ts to make nulls 0s")
    null_filled_ts = shift_ts.fillna(interpolators.fill(0.0))
    print(null_filled_ts)

shifted ts to add nulls
TimeStamp: 0     Value: null
TimeStamp: 1     Value: null
TimeStamp: 2     Value: 1.0
TimeStamp: 3     Value: 2.0
TimeStamp: 4     Value: 3.0
TimeStamp: 5     Value: 4.0

filled ts to make nulls 0s
TimeStamp: 0     Value: 0.0
TimeStamp: 1     Value: 0.0
TimeStamp: 2     Value: 1.0
TimeStamp: 3     Value: 2.0
TimeStamp: 4     Value: 3.0
TimeStamp: 5     Value: 4.0

# Additive White Gaussian Noise (AWGN)
>>> noise_ts = ts.transform(transformers.awgn(mean=0.0,sd=.03))
>>> print(noise_ts)
TimeStamp: 0     Value: 0.9962378841388397
TimeStamp: 1     Value: 1.9681980879378596
TimeStamp: 2     Value: 3.0289374962174405
TimeStamp: 3     Value: 3.990728648807705
TimeStamp: 4     Value: 4.935338359740761

TimeStamp: 5     Value: 6.03395072999318

```
## Segmentation
{: #time-series-segmentation}

Segmentation or windowing is the process of splitting a time series into multiple segments. The time series library supports various forms of segmentation and allows creating user-defined segments as well.

- Window based segmentation

  This type of segmentation of a time series is based on user specified segment sizes. The segments can be record based or time based. There are options that allow for creating tumbling as well as sliding window based segments.
  ```python
  >>> import tspy
  >>> ts_orig = tspy.builder()
     .add(tspy.observation(1,1.0))
     .add(tspy.observation(2,2.0))
     .add(tspy.observation(6,6.0))
     .result().to_time_series()
  >>> ts_orig
  TimeStamp: 1     Value: 1.0
  TimeStamp: 2     Value: 2.0
  TimeStamp: 6     Value: 6.0

  >>> ts = ts_orig.segment_by_time(3,1)
  >>> ts
  TimeStamp: 1     Value: original bounds: (1,3) actual bounds: (1,2) observations: [(1,1.0),(2,2.0)]
  TimeStamp: 2     Value: original bounds: (2,4) actual bounds: (2,2) observations: [(2,2.0)]
  TimeStamp: 3     Value: this segment is empty
  TimeStamp: 4     Value: original bounds: (4,6) actual bounds: (6,6) observations: [(6,6.0)]
  ```
- Anchor based segmentation

  Anchor based segmentation is a very important type of segmentation that creates a segment by anchoring on a specific lambda, which can be a simple value. An example is looking at events that preceded a 500 error or examining values after  observing an anomaly. Variants of anchor based segmentation include providing a range with multiple markers.
  ```python
  >>> import tspy
  >>> ts_orig = tspy.time_series([1.0, 2.0, 3.0, 4.0, 5.0])
  >>> ts_orig
  TimeStamp: 0     Value: 1.0
  TimeStamp: 1     Value: 2.0
  TimeStamp: 2     Value: 3.0
  TimeStamp: 3     Value: 4.0
  TimeStamp: 4     Value: 5.0

  >>> ts = ts_orig.segment_by_anchor(lambda x: x % 2 == 0, 1, 2)
  >>> ts
  TimeStamp: 1     Value: original bounds: (0,3) actual bounds: (0,3) observations: [(0,1.0),(1,2.0),(2,3.0),(3,4.0)]
  TimeStamp: 3     Value: original bounds: (2,5) actual bounds: (2,4) observations: [(2,3.0),(3,4.0),(4,5.0)]
  ```
- Segmenters

  There are several specialized segmenters provided out of the box by importing the `segmenters` package (using `from tspy.functions import segmenters`). An example segmenter is one that uses regression to segment a time series:
  ```python
  >>> ts = tspy.time_series([1.0,2.0,3.0,4.0,5.0,2.0,1.0,-1.0,50.0,53.0,56.0])
  >>> max_error = .5
  >>> skip = 1
  >>> reg_sts = ts.to_segments(segmenters.regression(max_error,skip,use_relative=True))
  >>> reg_sts

  TimeStamp: 0     Value:   range: (0, 4)   outliers: {}
  TimeStamp: 5     Value:   range: (5, 7)   outliers: {}
  TimeStamp: 8     Value:   range: (8, 10)   outliers: {}
  ```

## Reducers
{: #time-series-reducers}

A reducer is a function that is applied to the values across a set of time series to produce a single value. The time series `reducer` functions are similar to the reducer concept used by Hadoop/Spark. This single value can be a collection, but more generally is a single object. An example of a reducer function is averaging the values in a time series.

Several  `reducer` functions are supported, including:

- Distance reducers

  Distance reducers are a class of reducers that compute the distance between two time series. The library supports numeric as well as categorical distance functions on sequences. These include time warping distance measurements such as Itakura  Parallelogram, Sakoe-Chiba Band, DTW non-constrained and DTW non-time warped contraints. Distribution distances such as Hungarian distance and Earth-Movers distance are also available.

  For categorical time series distance measurements, you can use  Damerau Levenshtein and Jaro-Winkler distance measures.
  ```python
  >>> from tspy.functions import *
  >>> ts = tspy.time_series([1.0, 2.0, 3.0, 4.0, 5.0, 6.0])
  >>> ts2 = ts.transform(transformers.awgn(sd=.3))
  >>> dtw_distance = ts.reduce(ts2,reducers.dtw(lambda obs1, obs2: abs(obs1.value - obs2.value)))
  >>> print(dtw_distance)
  1.8557981638880405
  ```
- Math reducers

  Several convenient math reducers for numeric time series are provided. These include basic ones such as average, sum, standard deviation, and moments. Entropy, kurtosis, FFT and variants of it, various correlations, and histogram are also included. A convenient basic summarization reducer is the `describe` function that provides basic information about the time series.
  ```python
  >>> from tspy.functions import *
  >>> ts = tspy.time_series([1.0, 2.0, 3.0, 4.0, 5.0, 6.0])
  >>> ts2 = ts.transform(transformers.awgn(sd=.3))
  >>> corr = ts.reduce(ts2, reducers.correlation())
  >>> print(corr)
  0.9938941942380525

  >>> adf = ts.reduce(reducers.adf())
  >>> print(adf)
  pValue: -3.45
  satisfies test: false

  >>> ts2 = ts.transform(transformers.awgn(sd=.3))
  >>> granger = ts.reduce(ts2, reducers.granger(1))
  >>> print(granger) #f_stat, p_value, R2
  -1.7123613937876463,-3.874412217575385,1.0
  ```
- Another basic reducer that is very useful for getting a first order understanding of the time series is the describe reducer. The following illustrates this reducer:
  ```python
  >>> desc = ts.describe()
  >>> print(desc)
  min inter-arrival-time: 1
  max inter-arrival-time: 1
  mean inter-arrival-time: 1.0
  top: null
  unique: 6
  frequency: 1
  first: TimeStamp: 0     Value: 1.0
  last: TimeStamp: 5     Value: 6.0
  count: 6
  mean:3.5
  std:1.707825127659933
  min:1.0
  max:6.0
  25%:1.75
  50%:3.5
  75%:5.25
  ```

## Temporal joins

The library includes functions for temporal joins or joining time series based on their timestamps. The join functions are similar to those in a database, including left, right, outer, inner, left outer, right outer joins, and so on. The following sample codes shows some of these join functions:
```python
# Create a collection of observations (materialized TimeSeries)
observations_left = tspy.observations(tspy.observation(1, 0.0), tspy.observation(3, 1.0), tspy.observation(8, 3.0), tspy.observation(9, 2.5))
observations_right = tspy.observations(tspy.observation(2, 2.0), tspy.observation(3, 1.5), tspy.observation(7, 4.0), tspy.observation(9, 5.5), tspy.observation(10, 4.5))

# Build TimeSeries from Observations
ts_left = observations_left.to_time_series()
ts_right = observations_right.to_time_series()

# Perform full join
ts_full = ts_left.full_join(ts_right)
print(ts_full)

TimeStamp: 1     Value: [0.0, null]
TimeStamp: 2     Value: [null, 2.0]
TimeStamp: 3     Value: [1.0, 1.5]
TimeStamp: 7     Value: [null, 4.0]
TimeStamp: 8     Value: [3.0, null]
TimeStamp: 9     Value: [2.5, 5.5]
TimeStamp: 10     Value: [null, 4.5]

# Perform left align with interpolation
ts_left_aligned, ts_right_aligned = ts_left.left_align(ts_right, interpolators.nearest(0.0))

print("left ts result")
print(ts_left_aligned)
print("right ts result")
print(ts_right_aligned)

left ts result
TimeStamp: 1     Value: 0.0
TimeStamp: 3     Value: 1.0
TimeStamp: 8     Value: 3.0
TimeStamp: 9     Value: 2.5
right ts result
TimeStamp: 1     Value: 0.0
TimeStamp: 3     Value: 1.5
TimeStamp: 8     Value: 4.0
TimeStamp: 9     Value: 5.5
```

## Forecasting

A key functionality provided by the time series library is forecasting. The library includes functions for simple as well as complex forecasting models, including ARIMA, Exponential, Holt-Winters, and BATS. The following example shows the function to create a Holt-Winters:
```python
import random

model = tspy.forecasters.hws(samples_per_season=samples_per_season, initial_training_seasons=initial_training_seasons)

for i in range(100):
    timestamp = i
    value = random.randint(1,10) * 1.0
    model.update_model(timestamp, value)

print(model)

Forecasting Model
  Algorithm: HWSAdditive=5 (aLevel=0.001, bSlope=0.001, gSeas=0.001) level=6.087789839896166, slope=0.018901997884893912, seasonal(amp,per,avg)=(1.411203455586738,5, 0,-0.0037471500727535465)

#Is model init-ed
if model.is_initialized():
    print(model.forecast_at(120))

6.334135728495107

ts = tspy.time_series([float(i) for i in range(10)])

print(ts)

TimeStamp: 0     Value: 0.0
TimeStamp: 1     Value: 1.0
TimeStamp: 2     Value: 2.0
TimeStamp: 3     Value: 3.0
TimeStamp: 4     Value: 4.0
TimeStamp: 5     Value: 5.0
TimeStamp: 6     Value: 6.0
TimeStamp: 7     Value: 7.0
TimeStamp: 8     Value: 8.0
TimeStamp: 9     Value: 9.0

num_predictions = 5
model = tspy.forecasters.auto(8)
confidence = .99

predictions = ts.forecast(num_predictions, model, confidence=confidence)

print(predictions.to_time_series())

TimeStamp: 10     Value: {value=10.0, lower_bound=10.0, upper_bound=10.0, error=0.0}
TimeStamp: 11     Value: {value=10.997862810553725, lower_bound=9.934621260488143, upper_bound=12.061104360619307, error=0.41277640121597475}
TimeStamp: 12     Value: {value=11.996821082897318, lower_bound=10.704895525154571, upper_bound=13.288746640640065, error=0.5015571318964149}
TimeStamp: 13     Value: {value=12.995779355240911, lower_bound=11.50957896664928, upper_bound=14.481979743832543, error=0.5769793776877866}
TimeStamp: 14     Value: {value=13.994737627584504, lower_bound=12.33653268707341, upper_bound=15.652942568095598, error=0.6437557559526337}

print(predictions.to_time_series().to_df())

timestamp      value  lower_bound  upper_bound     error
0         10  10.000000    10.000000    10.000000  0.000000
1         11  10.997863     9.934621    12.061104  0.412776
2         12  11.996821    10.704896    13.288747  0.501557
3         13  12.995779    11.509579    14.481980  0.576979
4         14  13.994738    12.336533    15.652943  0.643756
```

## Time series SQL
{: #time-series-sql}

The time series library is tightly integrated with Apache Spark. By using new data types in Spark Catalyst, you are able to perform time series SQL operations that scale out horizontally using Apache Spark. This enables you to easily use time series extensions in {{site.data.keyword.iae_full_notm}} or in solutions that include {{site.data.keyword.iae_full_notm}} functionality like the {{site.data.keyword.DSX_short}} Spark environments.

SQL extensions cover most aspects of the time series functions, including segmentation, transformations, reducers, forecasting, and I/O. See [Analyzing time series data](https://cloud.ibm.com/docs/sql-query?topic=sql-query-ts_intro).
