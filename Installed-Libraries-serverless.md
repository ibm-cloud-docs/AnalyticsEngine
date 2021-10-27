---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-18"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Spark kernels and libraries
{: #installed-libs-serverless}

The {{site.data.keyword.iae_full_notm}} instance comes with a set of libraries depending on the chosen template. These libraries are pre-installed in each instance and the libraries are available by default on the kernels. The table below lists the locations of these libraries:

| Environment | Kernel | Libraries |                 
|-------------|--------|-----------|
| Python 3.7 |Python 3.7 with Spark 2.3.2 |Python libraries packaged with Anaconda3-2018.12 at `/home/common/conda/miniconda3.7` |
| R 3.6 | xxx | xxx|
| Scala 2.11|Scala 2.11 with Spark 2.3.2 |Scala/Java libraries (Scala 2.11 and Java 1.8) under `/home/common/lib/scala/spark2` |

## Python

To see the list of installed packages, execute the following command from a Python notebook:

```python
!pip list
```

Alternately, SSH to the instance and run this command:

```python
/home/common/conda/miniconda3.7/bin/pip list
```

## R

R version 3.4.x is installed with the base and all recommended packages corresponding to the release. In addition, a number of common data science R packages are installed and placed in the `/home/common/lib/R` directory.

The `/home/common/lib/R` directory serves the purpose of what is referred to as [R_LIBS_USER](https://stat.ethz.ch/R-manual/R-devel/library/base/html/libPaths.html) in R.

Packages for database interfaces like `DBI`, `RJDBC`, `RODBC`, `RMySQL`, development tools like `devtools` and data science packages like `sparklyr` and [OHDSI suite](https://github.com/OHDSI/) are made available.

To get a detailed description of the installed packages you can use the following commands:

- System command after SSHing to the cluster: `Rscript -e "installed.packages(lib.loc='/home/common/lib/R')"`
- From within an R notebook: `installed.packages(lib.loc='/home/common/lib/R')`

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
