---

copyright:
  years: 2017
lastupdated: "2017-07-12"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Lazy Spark initialization

When a kernel is acquired on the IBM Analytics Engine cluster, handles to Spark context, Spark SQL context, and Spark session etc are provided. A client application can safely assume these handles exist when a kernel is acquired by it and so can reference them directly in it's code for interacting with the Spark cluster. This makes using the cluster simpler for applications as they can remain independent of the specifics around how a Spark session needs to be initialized. 

With Lazy Spark initialisation a kernel is created and returned to a client with handles to  SparkContext, SQLContext, and SparkSession. However, these handles become active only when the application attempts to execute some code on the kernel. This means that the initialisation of the Spark session is delayed until code execution. This speeds up time taken for kernel creation while adding a little extra code execution time later on when the Spark session is activated.

## R kernel

With the R kernel, the Spark session is not initialized until code is executed that *needs* to access Spark. This means that you can continue to execute R code on the kernel and Spark is not initialized until your code directly or indirectly references a Spark session.

The first time that code is executed on the R kernel that requires Spark to be initialized, the application kernel outputs messages on the `stderr` stream of the execution request about Spark getting initialized. The following message is output when the kernel begins Spark initialisation: 

```
Obtaining Spark session...
```

Once Spark is successfully initialized, the following message is output:

```
Spark session obtained.
```

When working in a notebook, the notebook may display these messages when you execute a cell that requires Spark session to be initialized.

## Python kernel

The Python kernel initialises Spark whenever the first code execution request is submitted to it. In a notebook, this means the moment when the first cell is executed after launching a notebook. If Spark is initialized as a result of code being submitted for execution, the kernel outputs messages on the `stdout` stream of the request to indicate it is initialising a Spark session. The kernel outputs the following message when it begins Spark initialisation:

```
Waiting for a Spark session to start...
```

If after 5 minutes Spark is not initialized, it outputs the following message: 

```
Still waiting for Spark session to start. Request could be waiting with YARN for containers needed to begin the Spark session. Freeing up YARN resources may help. Continuing to wait for Spark session to start...
```

## Scala kernel

The Scala kernel currently does not support Lazy Spark initialization.  It is however expected that in future such support will be available.
