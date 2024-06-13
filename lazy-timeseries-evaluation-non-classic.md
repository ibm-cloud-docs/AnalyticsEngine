---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

subcollection: AnalyticsEngine

---


{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Lazy evaluation
{: #lazy-evaluation-non-classic}

Lazy evaluation is an evaluation strategy that delays the evaluation of an expression until its value is needed. When combined with memoization, lazy evaluation strategy avoids repeated evaluations and can reduce the running time of certain functions by a significant factor.

The time series library uses lazy evaluation to process data. Notionally an execution graph is constructed on time series data whose evaluation is triggered only when its output is materialized. Assuming an object is moving in a one dimensional space, whose location is captured by x(t). You can determine the harsh acceleration/braking (`h(t)`) of this object by using its velocity (`v(t)`) and acceleration (`a(t)`) time series as follows:

```python
# 1d location timeseries
x(t) = input location timeseries

# velocity - first derivative of x(t)
v(t) = x(t) - x(t-1)

# acceleration - second derivative of x(t)
a(t) = v(t) - v(t-1)

# harsh acceleration/braking using thresholds on acceleration
h(t) = +1 if a(t) > threshold_acceleration
     = -1 if a(t) < threshold_deceleration
     = 0 otherwise
```

This results in a simple execution graph of the form:

```bash
x(t) --> v(t) --> a(t) --> h(t)
```

Evaluations are triggered only when an action is performed, such as `compute h(5...10)`, i.e. `compute h(5), ..., h(10)`. The library captures narrow temporal dependencies between time series. In this example, `h(5...10)` requires `a(5...10)`, which in turn requires `v(4...10)`, which then requires `x(3...10)`. Only the relevant portions of `a(t)`, `v(t)` and `x(t)` are evaluated.

```bash
h(5...10) <-- a(5...10) <-- v(4...10) <-- x(3...10)
```
Furthermore, evaluations are memoized and can thus be reused in subsequent actions on `h`. For example, when a request for `h(7...12)` follows a request for `h(5...10)`, the memoized values `h(7...10)` would be leveraged; further, `h(11...12)` would be evaluated using `a(11...12), v(10...12)` and `x(9...12)`, which would in turn leverage `v(10)` and `x(9...10)` memoized from the prior computation.

In a more general example, you could define a smoothened velocity timeseries as follows:

```python
# 1d location timeseries
x(t) = input location timeseries

# velocity - first derivative of x(t)
v(t) = x(t) - x(t-1)

# smoothened velocity
# alpha is the smoothing factor
# n is a smoothing history
v_smooth(t) =  (v(t)*1.0 + v(t-1)*alpha + ... + v(t-n)*alpha^n) / (1 + alpha + ... + alpha^n)

# acceleration - second derivative of x(t)
a(t) = v_smooth(t) - v_smooth(t-1)
```

In this example `h(l...u)` has the following temporal dependency. Evaluation of `h(l...u)` would strictly adhere to this temporal dependency with memoization.

```bash
h(l...u) <-- a(l...u) <-- v_smooth(l-1...u) <-- v(l-n-1...u) <-- x(l-n-2...u)
```

## An Example
{: #time-series-example}

The following example shows a python code snippet that implements harsh acceleration on a simple in-memory time series. The library includes several built-in transforms. In this example the difference transform is applied twice to the location time series to compute acceleration time series. A map operation is applied to the acceleration time series using a harsh lambda function, which is defined after the code sample, that maps acceleration to either `+1` (harsh acceleration), `-1` (harsh braking) and `0` (otherwise). The filter operation selects only instances wherein either harsh acceleration or harsh braking is observed. Prior to calling `get_values`, an execution graph is created, but no computations are performed. On calling `get_values(5, 10)`, the evaluation is performed with memoization on the narrowest possible temporal dependency in the execution graph.

```python
import sparktspy as tspy
from tspy.functions import transformers

x = tspy.time_series([1.0, 2.0, 4.0, 7.0, 11.0, 16.0, 22.0, 29.0, 28.0, 30.0, 29.0, 30.0, 30.0])
v = x.transform(transformers.difference())
a = v.transform(transformers.difference())
h = a.map(harsh).filter(lambda h: h != 0)

print(h[5, 10])
```

The harsh lambda is defined as follows:

```python
def harsh(a):
    threshold_acceleration = 2.0
    threshold_braking = -2.0

    if (a > threshold_acceleration):
        return +1
    elif (a < threshold_braking):
        return -1
    else:
        return 0
```

## Learn more
{: #lazy-evaluation-non-classic-1}

To use the `tspy` Python SDK, see the [`tspy` Python SDK documentation](https://ibm-cloud.github.io/tspy-docs/).
