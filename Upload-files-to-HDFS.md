---

copyright:
  years: 2017
lastupdated: "2017-07-27"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Uploading files to HDFS

You can choose to upload your data in HDFS or Object Store.

**Remember:** You can upload files to your home directory (/user/clsadmin). It is not recommended that you upload files under the /tmp directory.

## Uploading your data by using the webhdfs REST API

For programmatic access to the HDFS, use the webhdfs REST API.

**To upload your data to the HDFS by using the webhdfs REST API**

**Pre-requisites:** Obtain the user credentials and the webhdfs URL from the [service credentials and end points](./Retrieve-service-credentials-and-service-end-points.html) of your service instance.

1. Open a command prompt.
2. Change directory to the location of the data files that you want to upload.
Using the webhdfs URL that is identified above, make a REST API call by using the cURL command to show directory contents, create directories, and upload files. For example, to show the current contents of your cluster's top-level HDFS directory that is named /user, run the following command:

```
curl -i -s --user clsadmin:your_password --max-time 45 \
 https://XXXXX:8443/gateway/default/webhdfs/v1/user?op=LISTSTATUS
```
{: codeblock}

The value of XXXXX is the host name of your cluster retrieved from the service end points json. If the call completes successfully, JSON returns `200 OK`.

**To upload a file**

* Run the following command:

```
curl -i -L  -s --user clsadmin:your_password --max-time 45 -X PUT -T file_name.txt \
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_file/file_name?op=CREATE
```
{: codeblock}

If the directories do not exist, they are created. If the call completes successfully, JSON returns `201 Created`.

Run more cURL commands, one for each file that you want to upload.

**To create an empty directory**

* If you want to create an output directory, for example, run the following command:

```
curl -i  -s --user clsadmin:your_password --max-time 45 -X PUT
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_directory?op=MKDIRS
```
{: codeblock}

**To remove a file**

* Run the following command:

```
curl -i -s --user clsadmin:your_password --max-time 45 -X DELETE
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_file?op=DELETE
```
{: codeblock}

You cannot remove a directory that is not empty.

An alternative way to look at the directory structure, contents, owners, and size is to navigate to the following URL:

```
https://XXXXX.services.us-south.bluemix.net:8443/gateway/default/hdfs/explorer.html
```

For more information, see the [webhdfs REST API documentation](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-hdfs/WebHDFS.html).
