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

# Using time reference system
{: #time-reference-system}

Time reference system (TRS) is a local, regional or global system used to identify time. A time reference system defines a specific projection for forward and reverse mapping between a timestamp and its numeric representation. A common example that most users are familiar with is UTC time, which maps a timestamp, for example, (1 Jan 2019, 12 midnight (GMT) into a 64-bit integer value  (1546300800000), which captures the number of milliseconds that have elapsed since 1 Jan 1970, 12 midnight (GMT). Generally speaking, the timestamp value is better suited for human readability, while the numeric representation is better suited for machine processing.

In the time series library, a time series can be associated with a TRS. A TRS is composed of a:
- Time tick that captures time granularity, for example 1 minute
- Zoned date time that captures a start time, for example `1 Jan 2019,  12 midnight US Eastern Daylight Savings time (EDT)`. A timestamp is mapped into a numeric representation by computing the number of elapsed time ticks since the start time. A numeric representation is scaled by the time tick and shifted by the start time when it is mapped back to a timestamp.

Note that this forward + reverse projection might lead to time loss. For instance, if the true time granularity of a time series is in seconds, then forward and reverse mapping of the time stamps `09:00:01` and `09:00:02` (to be read as `hh:mm:ss`) to a time tick of one minute would result in the time stamps `09:00:00` and `09:00:00` respectively. In this example, a time series, whose granularity is in seconds, is being mapped to minutes and thus the reverse mapping looses information. However, if the mapped granularity is higher than the granularity of the input time series (more specifically, if the time series granularity is an integral multiple of the mapped granularity) then the forward + reverse projection is guaranteed to be lossless. For example, mapping a time series, whose granularity is in minutes, to seconds and reverse projecting it to minutes would result in lossless reconstruction of the timestamps.

## Setting TRS
{: #setting-trs}

When a time series is created, it is associated with a TRS (or None if no TRS is specified). If the TRS is None, then the numeric values cannot be mapped to timestamps. Note that TRS can only be set on a time series at construction time. The reason is that a  time series by design is an immutable object. Immutability comes in handy when the library is used in multi-threaded environments or in distributed computing environments such as Apache Spark. While a TRS can be set only at construction time, it can be changed using the `with_trs` method as described in the next section. `with_trs` produces a new time series and thus has no impact on immutability.

Let us consider a simple time series created from an in-memory list:

```python
values = [1.0, 2.0, 4.0]
x = tspy.time_series.list(values)
x
```
This returns:
```
TimeStamp: 0     Value: 1.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 4.0
```

At construction time, the time series can be associated with a TRS. Associating a TRS with a time series allows its numeric timestamps to be as per the time tick and offset/timezone. The following example shows `1 minute and 1 Jan 2019, 12 midnight (GMT)`:

```python
zdt = datetime.datetime(2019,1,1,0,0,0,0,tzinfo=datetime.timezone.utc)
x_trs = tspy.time_series.list(data, time_tick=datetime.timedelta(minutes=1), start_time=zdt)
x_trs
```
This returns:
```
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:01Z     Value: 2.0
TimeStamp: 2019-01-01T00:02Z     Value: 4.0
```

Here is another example where the numeric timestamps are reinterpreted with a time tick of one hour and offset/timezone as `1 Jan 2019, 12 midnight US Eastern Daylight Savings time (EDT)`.

```python
tz_edt = datetime.timezone.edt
zdt = datetime.datetime(2019,1,1,0,0,0,0,tzinfo=tz_edt)
x_trs = tspy.time_series.list(data, time_tick=datetime.timedelta(hours=1), start_time=zdt)
x_trs
```
This returns:
```
TimeStamp: 2019-01-01T00:00-04:00     Value: 1.0
TimeStamp: 2019-01-01T00:01-04:00     Value: 2.0
TimeStamp: 2019-01-01T00:02-04:00     Value: 4.0
```

Note that the timestamps now indicate an offset of -4 hours from GMT (EDT timezone) and captures the time tick of one hour. Also note that setting a TRS does NOT change the numeric timestamps - it only specifies a way of interpreting numeric timestamps.

```python
x_trs.print(human_readable=False)
```
This returns:
```
TimeStamp: 0     Value: 1.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 4.0
```

## Changing TRS
{: #changing-trs}

You can change the TRS associated with a time series using the  `with_trs` function. Note that this function will throw an exception if the input time series is not associated with a TRS (if TRS is None). Using `with_trs` changes the numeric timestamps.

The following code sample shows TRS set at contructions time without using `with_trs`:

```python
# 1546300800 is the epoch time in seconds for 1 Jan 2019, 12 midnight GMT
zdt1 = datetime.datetime(1970,1,1,0,0,0,0,tzinfo=datetime.timezone.utc)
y = tspy.observations.of(tspy.observation(1546300800, 1.0),tspy.observation(1546300860, 2.0), tspy.observation(1546300920,
    4.0)).to_time_series(time_tick=datetime.timedelta(seconds=1), start_time=zdt1)
y.print()
y.print(human_readable=False)
```
This returns:
```
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:01Z     Value: 2.0
TimeStamp: 2019-01-01T00:02Z     Value: 4.0

# TRS has been set during construction time - no changes to numeric timestamps
TimeStamp: 1546300800     Value: 1.0
TimeStamp: 1546300860     Value: 2.0
TimeStamp: 1546300920     Value: 4.0
```

The following example shows how to apply `with_trs` to change  `time_tick` to one minute and retain the original time offset (1  Jan 1970, 12 midnight GMT):
```python
y_minutely_1970 = y.with_trs(time_tick=datetime.timedelta(minutes=1), start_time=zdt1)
y_minutely_1970.print()
y_minutely_1970.print(human_readable=False)
```
This returns:
```
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:01Z     Value: 2.0
TimeStamp: 2019-01-01T00:02Z     Value: 4.0

# numeric timestamps have changed to number of elapsed minutes since 1 Jan 1970, 12 midnight GMT
TimeStamp: 25771680     Value: 1.0
TimeStamp: 25771681     Value: 2.0
TimeStamp: 25771682     Value: 4.0
```
Now apply `with_trs` to change `time_tick` to one minute and the offset to 1 Jan 2019, 12 midnight GMT:
```python
zdt2 = datetime.datetime(2019,1,1,0,0,0,0,tzinfo=datetime.timezone.utc)
y_minutely = y.with_trs(time_tick=datetime.timedelta(minutes=1), start_time=zdt2)
y_minutely.print()
y_minutely.print(human_readable=False)
```
This returns:
```
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:01Z     Value: 2.0
TimeStamp: 2019-01-01T00:02Z     Value: 4.0

# numeric timestamps are now minutes elapsed since 1 Jan 2019, 12 midnight GMT
TimeStamp: 0     Value: 1.0
TimeStamp: 1     Value: 2.0
TimeStamp: 2     Value: 4.0
```

To better understand how it impacts post processing, let's examine the following. Note that `get_values` on numeric timestamps  operates on the underlying numeric timestamps associated with the time series.

```python
print(y.get_values(0,2))
print(y_minutely_1970.get_values(0,2))
print(y_minutely.get_values(0,2))
```
This returns:
```
# numeric timestamps in y are in the range 1546300800, 1546300920 and thus y.get_values(0,2) is empty
[]
# numeric timestamps in y_minutely_1970 are in the range 25771680, 25771682 and thus y_minutely_1970.get_values(0,2) is empty
[]
# numeric timestamps in y_minutely are in the range 0, 2
[(0,1.0),(1,2.0),(2,4.0)]
```
The method `get_values` can also be applied to datetime objects. This results in an exception if the underlying time series is not associated with a TRS (if TRS is None). Assuming the underlying time series has a TRS, the datetime objects are mapped to a numeric range using the TRS.

```python
# Jan 1 2019, 12 midnight GMT
dt_beg = datetime.datetime(2019,1,1,0,0,0,0,tzinfo=datetime.timezone.utc)
# Jan 1 2019, 12:02 AM GMT
dt_end = datetime.datetime(2019,1,1,0,2,0,0,tzinfo=datetime.timezone.utc)

print(y.get_values(dt_beg, dt_end))
print(y_minutely_1970.get_values(dt_beg, dt_end))
print(y_minutely.get_values(dt_beg, dt_end))

# get_values on y in UTC millis
[(1546300800,1.0),(1546300860,2.0), (1546300920,4.0)]
# get_values on y_minutely_1970 in UTC minutes
[(25771680,1.0),(25771681,2.0),(25771682,4.0)]
# get_values on y_minutely in minutes offset by 1 Jan 2019, 12 midnight
[(0,1.0),(1,2.0),(2,4.0)]
```

## Duplicate timestamps
{: #duplicate-timestamps}

Changing the TRS can result in duplicate timestamps. The following example changes the time tick to one hour which results in duplicate timestamps. The time series library handles duplicate timestamps seamlessly and provides convenience combiners to reduce values associated with duplicate timestamps into a single value,  for example by calculating an average of the values grouped by duplicate timestamps.

```python
y_hourly = y_minutely.with_trs(time_tick=datetime.timedelta(hours=1), start_time=zdt2)
print(y_minutely)
print(y_minutely.get_values(0,2))

print(y_hourly)
print(y_hourly.get_values(0,0))
```
This returns:
```
# y_minutely - minutely time series
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:01Z     Value: 2.0
TimeStamp: 2019-01-01T00:02Z     Value: 4.0

# y_minutely has numeric timestamps 0, 1 and 2
[(0,1.0),(1,2.0),(2,4.0)]

# y_hourly - hourly time series has duplicate timestamps
TimeStamp: 2019-01-01T00:00Z     Value: 1.0
TimeStamp: 2019-01-01T00:00Z     Value: 2.0
TimeStamp: 2019-01-01T00:00Z     Value: 4.0

# y_hourly has numeric timestamps of all 0
[(0,1.0),(0,2.0),(0,4.0)]
```
Duplicate timestamps can be optionally combined as follows:
```python
y_hourly_averaged = y_hourly.transform(transformers.combine_duplicate_time_ticks(lambda x: sum(x)/len(x))
print(y_hourly_averaged.get_values(0,0))
```
This returns:
```
# values corresponding to the duplicate numeric timestamp 0 have been combined using average
# average = (1+2+4)/3 = 2.33
[(0,2.33)]
```

## Learn more

To use the `tspy` Python SDK, see the [`tspy` Python SDK documentation](https://ibm-cloud.github.io/tspy-docs/).
