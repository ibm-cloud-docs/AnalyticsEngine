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

# file-system
## Description

Interact with HDFS on the IBM Analytics Engine cluster.

## Usage

```
bx ae file-system [--user <user>] [--password <password>] get SRC DST
  SRC is the HDFS file
  DST is the local file

bx ae file-system [--user <user>] [--password <password>] ls [DIR]
  DIR is the optional HDFS directory

bx ae file-system [--user <user>] [--password <password>] mkdir DIR
  DIR is the HDFS directory

bx ae file-system [--user <user>] [--password <password>] mv SRC DST
  SRC and DST are the HDFS file or directory

bx ae file-system [--user <user>] [--password <password>]  SRC DST
  SRC is the local file
  DST is the HDFS file

bx ae file-system [--user <user>] [--password <password>] rm [-R] FILE

  FILE is the HDFS file or directory
  -R, --R remove directories and their contents recursively
```

## Options

Flag       | Description
---------- | ----------------------------------------------------
--user     | A user with authority to get the version information
--password | The password for the selected user

## Sub Commands

- [get](#get)
- [ls](#ls)
- [mkdir](#mkdir)
- [mv](#mv)
- [put](#put)
- [rm](#rm)

### Get

Copies a single file to the local file system from a remote HDFS file system.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] get [SRC] [DST]
```

Args:

- `SRC` is the file path remote HDFS file system
- `DST` is the file path on local file system

Example:

```
$ bx ae file-system get /user/clsadmin/run.log run.log
User (clsadmin)>
Password>
Copying HDFS file '/user/clsadmin/run.log' contents to 'run.log'...
OK
```

### ls

Lists contents of remote destination directory.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] ls [DIR]
```

Args:

- `DIR` is path of remote HDFS directory. It's optional (if empty contents of HDFS user home directory are returned)

Example:

```
$ bx ae file-system ls
User (clsadmin)>
Password>
Found 2 files in '/user/clsadmin'...
drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:28 CDT 2017  .sparkStaging
drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:11 CDT 2017  cli
OK
```

```
$ bx ae file-system ls /user/clsadmin/cli
User (clsadmin)>
Password>
Found 3 files in '/user/clsadmin/cli'...
drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 11:19:36 CDT 2017  0fdd1caf-a21f-4a18-970c-e40255e8f0ad
drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 11:19:16 CDT 2017  28c3a243-59cc-4bda-a186-a5bd77f62570
drwxr-xr-x  0  clsadmin  biusers  0  Tue Apr 11 12:33:11 CDT 2017  5b9a0a4c-21c1-4b41-b8bb-2b9ca47e1a51
OK
```

### mkdir

Creates a directory on remote HDFS file system.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] mkdir [DIR]
```

Args:

- `DIR` is the path on HDFS on file system

Example:

```
$ bx ae file-system mkdir /user/clsadmin/jobs
User (clsadmin)>
Password>
Creating HDFS directory '/user/clsadmin/jobs'...
OK
```

### mv

Moves a single file from source to destination on remote HDFS file system.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] mv [SRC] [DST]
```

Args:

- `SRC` and `DST` are file paths on remote HDFS file system

Example:

```
$ bx ae file-system mv /user/clsadmin/run.log /user/clsadmin/run.log.bkp
User (clsadmin)>
Password>
Moving HDFS path from '/user/clsadmin/run.log' to '/user/clsadmin/run.log.bkp'...
OK
```

```
$ bx ae file-system mv /user/clsadmin/run.log /tmp/run.log.bkp
User (clsadmin)>
Password>
Moving HDFS path from '/user/clsadmin/run.log' to '/tmp/run.log.bkp'...
OK
```

### put

Copies a single file from local file system to remote HDFS file system.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] put [SRC] [DST]
```

Args:

- `SRC` is the file path on the local file system
- `DST` is the file path on remote HDFS file system

Example:

```
$ bx ae file-system put sparkpi_2.10-1.0.jar /user/clsadmin/jobs/sparkpi_2.10-1.0.jar
User (clsadmin)>
Password>
Copying local file 'sparkpi_2.10-1.0.jar' to HDFS location '/user/clsadmin/jobs/sparkpi_2.10-1.0.jar'...
OK
```

### rm

Removes a file or directory from remote HDFS.

Usage:

```
bx ae file-system [--user <user>] [--password <password>] rm [-R] FILE
```

Args:

- `FILE` is the file path on remote HDFS file system

Options:

- `-R` remove directory recursively

Example:

Removes a file.

```
$ bx ae file-system rm /user/clsadmin/run.log
User (clsadmin)>
Password>
Are you sure you want to remove '/user/clsadmin/run.log' from HDFS ? [y/N]> y
Removing...
OK
```

Removes directory recursively.

```
$ bx ae file-system rm -R /user/clsadmin/logs
User (clsadmin)>
Password>
Are you sure you want to remove '/user/clsadmin/logs' from HDFS ? [y/N]> y
Removing...
OK
```
