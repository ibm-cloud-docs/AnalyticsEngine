---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Installing additional libraries

In addition to the libraries that come pre-installed, there may be a need to use additional user libraries.

## Cluster wide installation

For distributed operations such as Spark jobs that execute on any node of the cluster, the dependency libraries need to be available on all nodes of the cluster. Refer the instructions [here](./customizing-cluster.html) for such cluster wide installation of libraries through customization scripts.

Note that the customization scripts should install libraries/packages into the same environments as listed below to make sure they get picked up by the JNBG service. The scripts need not rely only on public repositories like pypi, maven central, CRAN (or other publicly accessible URLs) to install the libraries/packages. Instead, you may choose to package your libraries/packages into archive files and place them in object storage, which the customization script can pull down and install. Refer to [this page](./Customization-script-on-Bluemix-Object-Store.html) for examples on how to place your scripts and related artefacts in an object store for customizing your cluster.

_Installation of libraries in this manner is permanent i.e, the libraries are always available to all interactive sessions by default._

### Python 2

To install Python 2.7 libraries, your script must install into the `/home/common/conda/anaconda2` environment by using:

 ```
 /home/common/conda/anaconda2/bin/pip install <package-name>
 ```

 And if installing from a local or remote archive:

 ```
 /home/common/conda/anaconda2/bin/pip install <archive url or local file path>
 ```

### Python 3

To install Python 3.5 libraries, your script must install into the `/home/common/conda/anaconda3` environment by using:

 ```
 /home/common/conda/anaconda3/bin/pip install <package-name>
 ```

 And if installing from a local or remote archive:

 ```
 /home/common/conda/anaconda3/bin/pip install <archive url or or local file path>
 ```

### Scala/Java

Scala/Java libraries must be copied into the following designated directories:

 * `/home/common/lib/scala/common` - Scala/Java libraries that are not Spark version specific
 * `/home/common/lib/scala/spark2` - Scala/Java libraries that are specific to Spark version 2.x

 Note that the Scala libraries should be compatible with Scala 2.11 and Java 1.8 as that is the runtime used by
 JNBG.

### R

R libraries must be installed into the `/home/common/lib/R` directory.

**To install the R package from an archive file**

1. Download the archive repository.

```
wget <path-to-archive>/<packagename>/<packagename>_<version>.tar.gz
```
{: codeblock}

2. Use the R command to install the package.

```
R CMD INSTALL -l /home/common/lib/R <packagename>_<version>.tar.gz
```
{: codeblock}

Example for installing the R package:
```
wget https://cran.r-project.org/src/contrib/Archive/ibmdbR/ibmdbR_1.48.0.tar.gz
R CMD INSTALL -l /home/common/lib/R ibmdbR_1.48.0.tar.gz
```
{: codeblock}

**To install an R package from a CRAN repository**

* Use the following command to install the R package:

```
R -e "install.packages('<package-name>', repos='<cran-repo-base-url>', lib='/home/common/lib/R')"
```
{: codeblock}

Example for installing an R package from a CRAN repository:
```
R -e "install.packages('ibmdbR', repos='https://cran.r-project.org/', lib='/home/common/lib/R')"
```
{: codeblock}

Note that in each case, the packages are installed into the `/home/common/lib/R` directory. This is important as otherwise, the R packages won't be available in your R notebook/Spark environments.


## Notebook / Interactive session specific installation

The Apache Toree based Scala kernel supports the `%AddDeps` and `%AddJar` line magics that can be used to add Scala/Java libraries to the running session.

Installation of libraries into the interactive session in this manner is temporary as the libraries are available only within the kernel session that executed the installation and are lost once the kernel stops.

`%AddDeps` takes maven coordinates of a library as an argument and can download transitive dependencies from a maven repository. For example:

```
%AddDeps org.joda joda-money 0.11 --transitive --trace --verbose
```

`%AddJar%` takes a URL pointing to a library Jar as an argument which gets downloaded and added to the environment. For example:

```
%AddJar https://repo1.maven.org/maven2/org/joda/joda-money/0.11/joda-money-0.11.jar
```
These libraries are made available to the executors.

For more details on these and other magics supported by the Apache Toree kernel, refer the tutorial notebook [here](https://github.com/apache/incubator-toree/blob/master/etc/examples/notebooks/magic-tutorial.ipynb).

## Local node installation

Python packages can be permanently installed on the host running the JNBG service by executing the following command in a Python notebook or interactive session.

```
!pip install <package-name>
```

Packages installed in this manner can be used in any code that is executed on the local (JNBG host) node, but not code that may get executed on other cluster nodes such as in distributed Spark processing.
