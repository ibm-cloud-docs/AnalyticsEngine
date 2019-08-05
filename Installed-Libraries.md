---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-09"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Spark kernels and libraries on the cluster
{: #installed-libs}

The {{site.data.keyword.iae_full_notm}} cluster comes with a set of libraries, which are pre-installed on each of the cluster nodes that are available by default on the kernels. The table below lists the locations of these libraries:

|AE version| Environment | Kernel | Libraries |                 
|-------------|--------|-----------|------------|
|AE 1.2| Python 3.7 |Python 3.7 with Spark 2.3.2 |Python libraries packaged with Anaconda3-2018.12 at /home/common/conda/anaconda3/ |
|AE 1.2| Scala 2.11|Scala 2.11 with Spark 2.3.2 |Scala/Java libraries (Scala 2.11 and Java 1.8) under /home/common/lib/scala/spark2 |
| AE 1.1| Python 2.7 | Python 2.7 with Spark 2.1 | Python libraries packaged with Anaconda2 4.3.0 at /home/common/conda/anaconda2/ |
|AE 1.1| Python 3.5 | Python 3.5 with Spark 2.1 | Python libraries packaged with Anaconda3 4.2.0 at /home/common/conda/anaconda3/|
|AE 1.1| Scala 2.11 | Scala 2.11 with Spark 2.1 | Scala/Java libraries (Scala 2.11 and Java 1.8) under /home/common/lib/scala/spark2 |

**Note:** Python 2 is available only on `AE 1.1` clusters. However, you are encouraged to write your applications in Python 3 as Python 2 will only be supported until the end of 2019.

For installed Spark connectors, see the [documentation](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-spark-connectors).

## Python

To see the list of installed packages, execute the following command from a Python notebook:

```python
!pip list
```

Alternately, SSH to the cluster and run this command:

```python
/home/common/conda/anaconda3/bin/pip list
```
**Special note for AE 1.1**

On `AE 1.1` clusters, both Python 2 and 3 are available. However, the default configuration of Spark Yarn jobs is Python 3. We encourage you to move all of your Spark applications to Python 3 soon because Python 2 will not be supported after the end of 2019.

If you want to set the configuration to Python 2, you have to update the following values in the custom spark-default.conf file:
```
"spark.yarn.appMasterEnv.PYSPARK_PYTHON":"/home/common/conda/anaconda2/bin/python",
"spark.executorEnv.PYSPARK_PYTHON":"/home/common/conda/anaconda2/bin/python"
```
{: codeblock}

To run spark-submit jobs in Python 2, you must set the environment variables for Python 2 as follows:

```python
export PATH=/home/common/conda/anaconda2/bin:$PATH
export PYSPARK_PYTHON=/home/common/conda/anaconda2/bin/python
export PYTHONPATH=~/pipAnaconda2Packages/
export PIP_CONFIG_FILE=/home/common/conda/anaconda2/etc/pip.conf
```
{: codeblock}

To see the list of Python 2 packages, you can SSH to the cluster and run the following command:

``` python
/home/common/conda/anaconda2/bin/pip list
```
{: codeblock}

## R

R version 3.4.x is installed with the base and all recommended packages corresponding to the release. In addition, a number of common data science R packages are installed and placed in the `/home/common/lib/R` directory.

The `/home/common/lib/R` directory serves the purpose of what is referred to as [R_LIBS_USER](https://stat.ethz.ch/R-manual/R-devel/library/base/html/libPaths.html) in R.

Packages for database interfaces like `DBI`, `RJDBC`, `RODBC`, `RMySQL`, development tools like `devtools` and data science packages like `sparklyr` and [OHDSI suite](https://github.com/OHDSI/) are made available.

To get a detailed description of the installed packages you can use the following commands:

* System command after SSHing to the cluster:

  `Rscript -e "installed.packages(lib.loc='/home/common/lib/R')"`

* From within an R notebook:

  `installed.packages(lib.loc='/home/common/lib/R')`

## Scala or Java

Scala or Java libraries are added to the following directory that is added to the CLASSPATH of Spark drivers and executors:

- `/home/common/lib/scala/common`: Scala or Java libraries that are not Spark version specific

- `/home/common/lib/scala/spark2`: Scala or Java libraries that are included by default in the Spark2 environment.

To view the list of libraries, simply list the content of these directories by issuing the corresponding commands in a notebook or on the command line after SSH-ing to the cluster.

For example, in a Scala notebook execute the below command to list the contents of the `/home/common/lib/scala/spark2` directory:
```
import sys.process._
"ls -al /home/common/lib/scala/spark2" !
```
{: codeblock}
