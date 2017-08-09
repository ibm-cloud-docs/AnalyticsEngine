---

copyright:
  years: 2017
lastupdated: "2017-07-23"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Installed libraries

The IBM Analytics Engine cluster comes with a set of libraries pre-installed on each of the cluster nodes that are available by default with corresponding kernels. The table below lists the locations of these libraries:

| Environment | Kernel | Libraries |                 
|-------------|--------|-----------|
| Python 2.7 | Python 2.7 with Spark 2.1 | Python libraries packaged with Anaconda2 4.3.0 at /home/common/conda/anaconda2/ |
| Python 3.5 | Python 3.5 with Spark 2.1 | Python libraries packaged with Anaconda3 4.2.0 at /home/common/conda/anaconda3/|
| Scala 2.11 | Scala 2.11 with Spark 2.1 | Scala/Java libraries (Scala 2.11 and Java 1.8) under  /home/common/lib/ and /home/common/lib/scala/common |

For installed Spark connectors refer to [this page](./supported-connectors.html).

## Python

Executing this command in a Python 2.7 or 3.5 notebook will list the installed packages:

```
!pip list
```
{: codeblock}

Alternately, SSH to the cluster and run this command to list Python 2.7 packages:
```
/home/common/conda/anaconda2/bin/pip list
```
{: codeblock}

and, to list Python 3.5 packages:
```
/home/common/conda/anaconda3/bin/pip list
```
{: codeblock}

## R

R version 3.3.x is installed with the base and all recommended packages corresponding to the release. In addition a number of common data science R packages are installed and placed in the `/home/common/lib/R` directory.

The `/home/common/lib/R` directory serves the purpose of what is referred to as [R_LIBS_USER](https://stat.ethz.ch/R-manual/R-devel/library/base/html/libPaths.html) in R.

Packages for database interfaces like `DBI`, `RJDBC`, `RODBC`, `RMySQL`, development tools like `devtools` and data science packages like `sparklyr` and [OHDSI suite](https://github.com/OHDSI/) are made available.

To get a detailed description of the installed packages you can use the following commands:

* System command line from SSHing to the cluster:

  `Rscript -e "installed.packages(lib.loc='/home/common/lib/R')"`

* From within an R notebook:

  `installed.packages(lib.loc='/home/common/lib/R')`

## Scala/Java
Scala/Java libraries are placed into the following directories that get added to the CLASSPATH of Spark drivers and executors:

* `/home/common/lib/scala/common` - Scala/Java libraries that are not Spark version specific
* `/home/common/lib/scala/spark2` - Scala/Java libraries that are specific to Spark version 2.x

To view the list of libraries, simply list the content of these directories by issuing the corresponding commands in a notebook or on the command line after SSH-ing to the cluster.

For example, in a Scala notebook executing the below command will list the contents of the `/home/common/lib/scala/common` directory:
```
import sys.process._
"ls -al /home/common/lib/scala/common" !
```
{: codeblock}
