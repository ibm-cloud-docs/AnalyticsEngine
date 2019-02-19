---

copyright:
  years: 2017, 2019
lastupdated: "2018-09-26"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# Uploading files to HDFS
{: #upload-files-hdfs}

You can choose to upload your data in HDFS or an object store. Data can be loaded into HDFS using the HDFS CLI or the WebHDFS API. For sensitive data, it is recommended to use a secure location that is previously created in HDFS.

You can upload files to your home directory (`/user/clsadmin`). However, you should avoid uploading files to the `/tmp` directory.

## Uploading your data by using the HDFS CLI

**Prerequisite**: Obtain the user credentials and ssh endpoint from cluster service credentials.

To use the HDFS CLI, SSH to the cluster using the credentials obtained earlier. You can access the HDFS CLI using the HDFS command. Refer to the following examples for using the HDFS CLI:

- **Creating an empty directory**

 To create a directory under the user home:

 ```hdfs dfs –mkdir –p /user/clsadmin/test-dir```

- **Uploading a file to HDFS**

 To upload a file to an existing HDFS directory:

 ```hdfs dfs –put test-file /user/clsadmin/test-dir```

- **Deleting a file/directory from HDFS**

 To delete file/directory from HDFS:

 ```hdfs dfs –rm –f /user/clsadmin/test-dir```

## Uploading your data by using the WebHDFS REST API

For programmatic access to the HDFS, use the WebHDFS REST API.

### Uploading your data to the HDFS by using the WebHDFS REST API

**Prerequisites:** Obtain the user credentials and the WebHDFS URL from the [service credentials and end points](/docs/services/AnalyticsEngine?topic=AnalyticsEngine-retrieve-credentials) of your service instance.

To upload data to HDFS by using the WebHDFS REST API:
1. Open a command prompt.
2. Change directory to the location of the data files that you want to upload. Using the WebHDFS URL that is identified above, make a REST API call by using the cURL command to show directory contents, create directories, and upload files. For example, to show the current contents of your cluster's top-level HDFS directory that is named `/user`, run the following command:

 ```
curl -i -s --user clsadmin:your_password --max-time 45 \
 https://XXXXX:8443/gateway/default/webhdfs/v1/user?op=LISTSTATUS
```
{: codeblock}

 The value of XXXXX is the host name of your cluster retrieved from the service end points json. If the call completes successfully, JSON returns `200 OK`.

1. To upload a file, run the following command:

 ```
curl -i -L -s --user clsadmin:your_password --max-time 45 -X PUT -T file_name.txt \
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_file/file_name?op=CREATE
```
{: codeblock}

 If the directories do not exist, they are created. If the call completes successfully, JSON returns `201 Created`.

 Run more cURL commands, one for each file that you want to upload.

1. To create an empty directory, for example an output directory, run the following command:

 ```
curl -i  -s --user clsadmin:your_password --max-time 45 -X PUT
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_directory?op=MKDIRS
```
{: codeblock}

1. To remove a file, run the following command:

 ```
curl -i -s --user clsadmin:your_password --max-time 45 -X DELETE
 https://XXXXX:8443/gateway/default/webhdfs/v1/user/clsadmin/path_to_file?op=DELETE
```
{: codeblock}

 You cannot remove a directory that is not empty.

An alternative way to look at the directory structure, contents, owners, and size is to navigate to the following URL:

```
https://<changeme>:8443/gateway/default/hdfs/explorer.html
```
where `<changeme>`  is the URL to the cluster. For example, for data on a cluster in the US-South region, use:
```
https://XXXXX.us-south.ae.appdomain.cloud:8443/gateway/default/hdfs/explorer.html
```
For more information, see the [WebHDFS REST API documentation](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-hdfs/WebHDFS.html).

## Working with encrypted data

Hadoop transparent data encryption is automatically enabled for your cluster. Your cluster comes with a predefined HDFS encryption zone, which is identified by the HDFS path/securedir. Files that are placed in the encryption zone are automatically encrypted. The files are automatically decrypted when they are accessed through various Hadoop client applications, such as HDFS shell commands, WebHDFS APIs, and the Ambari file browser.

Data encryption and decryption are transparent to your Hadoop applications. A file that is stored in the encryption zone can be referenced in Hadoop applications by specifying the file's complete HDFS path, just like any regular HDFS file. The data that is stored in the encryption zone is accessible only to an accredited user. It is important to remember that when you copy an encrypted file from `/securedir` to another location on the HDFS that is outside of the encryption zone, to your local file system, or to external storage such as object storage, the file in those locations is not encrypted.
