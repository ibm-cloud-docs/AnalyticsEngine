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

# Uploading files to HDFS

You can choose to upload your data in HDFS or Object Store.

* The easiest way to upload data to the HDFS is to use the Files view of the Ambari console. For programmatic access to the HDFS, use the WebHDFS REST API. 
* Uploading your data to an object store helps you to access the data from any of your IBM Analytics Engine clusters.

## Uploading your data by using the Ambari console file browser

You can upload your data to the HDFS by using the Ambari file browser.

To upload your data to the HDFS by using the Ambari files browser, complete the following steps:

From the Ambari console, click the Files Browser icon. The Ambari Files Browser view displays the contents of the root directory (/) by default.
Select the HDFS directory to which files will be uploaded. Click Upload, select one or more files from the local file system, and upload those files to the HDFS.

**Remember:** You can upload files to your home directory (/user/username). It is not recommended that you upload files under the /tmp directory.


## Uploading your data by using the WebHDFS REST API

For programmatic access to the HDFS, use the WebHDFS REST API.

To upload your data to the HDFS by using the WebHDFS REST API, complete the following steps:

**Pre-req:** Obtain the user credentials and the WebHDFS URL from the [service credentials and end points](./Retrieve-service-credentials-and-service-end-points.html) of your service instance. 

1. Open a command prompt.
2. Change directory to the location of the data files that you want to upload.
Using the WebHDFS URL that is identified above, make a REST API call by using the cURL command to show directory contents, create directories, and upload files. For example, to show the current contents of your cluster's top-level HDFS directory that is named /user, run the following command:

```
curl -i -k -s --user your_username:your_password --max-time 45
 https://XXXXX:8443/gateway/default/webhdfs/v1/user?op=LISTSTATUS
```
The value of XXXXX is the host name of your cluster retrieved from the service end points json. If the call completes successfully, JSON returns `200 OK`.

To upload a file, run the following command:
```
curl -i -L -k -s --user your_username:your_password --max-time 45 -X PUT -T file_name.txt
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/your_username/path_to_file/file_name?op=CREATE
```

If the directories do not exist, they are created. If the call completes successfully, JSON returns `201 Created`.

Run more cURL commands, one for each file that you want to upload.

To create an empty directory (for example, an output directory), run the following command:

```
curl -i -k -s --user your_username:your_password --max-time 45 -X PUT
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/your_username/path_to_directory?op=MKDIRS
```

To remove a file, run the following command:

```
curl -i -s --user your_username:your_password --max-time 45 -X DELETE
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/your_username/path_to_file?op=DELETE
```

You cannot remove a directory that is not empty. 

An alternative way to look at the directory structure, contents, owners, and size is to navigate to the following URL:

```
https://XXXXX.services.us-south.bluemix.net:8443/gateway/default/hdfs/explorer.html
```

For more information, see the [WebHDFS REST API documentation](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-hdfs/WebHDFS.html).
