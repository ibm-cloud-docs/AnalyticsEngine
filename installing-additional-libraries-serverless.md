---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-23"

subcollection: AnalyticsEngine

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Installing additional libraries
{: #install-additional-libs-serverless}

You might want to install other libraries in addition to the libraries that are  pre-installed on the instance.

## Cluster wide installation
{: #cluster-wide-installation}

For distributed operations such as Spark jobs that execute on any node of the cluster, the dependency libraries need to be available on all of the nodes of the cluster.

For example, if you need to install a Python package, you would intuitively execute the command `pip install <package-name>` after you SSH to the cluster. However, note that you need to execute this command on all the nodes of the cluster, not just the `mn003` node. If you want do this manually, you will need to SSH from `mn003` to each of the data nodes of the cluster (`dn001`, `dn002`, and so on). This can get tedious if you are working on a huge cluster, and even more so, if you need to repeat this step for every new cluster that you create. To solve this, you should use customization scripts that install the Python packages on all nodes of the cluster, either at the time the cluster is created  (bootstrap customization) or after cluster creation (adhoc customization). See [Installing libraries on all clusters by using customization scripts](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-cust-cluster).

Note that the customization scripts should install the libraries or packages into the same environments to ensure they get picked up by the JNBG service. The scripts need not rely only on public repositories like PyPi, Maven Central, CRAN (or other publicly accessible URLs) to install the libraries or packages. Instead, you can choose to package your libraries or packages into archive files and place them in Object Storage, which the customization script retrieves and installs. See [Examples of how to place your scripts and related artefacts in an object store for cluster customization](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-cust-examples).

Installing the libraries in this manner is permanent; the libraries are always available to all interactive sessions by default.

Note that you cannot use the `--user` option in `pip` install commands in {{site.data.keyword.iae_full_notm}}.

### Python 3
{: #install-python-3}

The Anaconda3 environment on `AE 1.2` clusters comes with Python 3.7. See [Installed libraries](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-installed-libs).

To install Python 3.x libraries, your script must install in the `conda3` environment by using:

 ```
 pip install <package-name>
 ```

 If you install from a local or remote archive, use:

 ```
 pip install <archive url or local file path>
 ```

Note that the additional libraries get installed under `~/pipAnaconda3Packages/`.


### Scala or Java
{: #install-scala-java}

Scala or Java libraries must be copied to the following designated directory:

 * `~/scala`: Scala or Java libraries

 Note that the Scala libraries should be compatible with Scala 2.11 and Java 1.8 as that is the runtime used by JNBG.

### R
{: #install-r}

R libraries must be installed to the `~/R` directory.

To install the R package from an archive file:

1. Download the archive repository:

 ```
wget <path-to-archive>/<packagename>/<packagename>_<version>.tar.gz
 ```

2. Use the R command to install the package:

 ```
R CMD INSTALL <packagename>_<version>.tar.gz
 ```

To install an R package from a CRAN repository:

1. Use the following command:
```
R -e "install.packages('<package-name>', repos='<cran-repo-base-url>')"
```

Note that in both cases, the packages are installed to the `~/R` directory. This is important as otherwise, the R packages won't be available in your R notebook and Spark environments.

## Notebook or interactive session specific installations

The Apache Toree based Scala kernel supports the `%AddDeps` and `%AddJar` line magics that can be used to add Scala and Java libraries to the running session.

Installing the libraries into an interactive session in this manner is temporary as the libraries are available only within the kernel session that executed the installation and are lost once the kernel stops.

`%AddDeps` takes maven coordinates of a library as an argument and can download transitive dependencies from a Maven repository. For example:

```
%AddDeps org.joda joda-money 0.11 --transitive --trace --verbose
```

`%AddJar%` takes a URL pointing to a library JAR as an argument which gets downloaded and added to the environment. For example:

```
%AddJar https://repo1.maven.org/maven2/org/joda/joda-money/0.11/joda-money-0.11.jar
```
These libraries are made available to the executors.

See this [tutorial on magics supported by the Apache Toree kernel](https://github.com/apache/incubator-toree/blob/master/etc/examples/notebooks/magic-tutorial.ipynb)  for more details.

## Local node installation

Python packages can be permanently installed on the host running the JNBG service by executing the following command in a Python notebook or interactive session.

```
!pip install <package-name>
```

Packages installed in this manner can be used in any code that is executed on the local (JNBG host) node, but not code that may get executed on other cluster nodes such as in distributed Spark processing.
