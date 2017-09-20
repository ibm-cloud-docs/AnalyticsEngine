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

# kernels
## Description
Interact with Jupyter Kernel Gateway (JKG) kernels on IBM Analytics Engine cluster.

## Usage

``` 
bx ae kernels [--user <user>] [--password <password>] create SPEC_NAME
    SPEC_NAME is the name of a kernel specification.
    Use 'bx ae kernels specs' for available kernel specification names.

bx ae kernels [--user <user>] [--password <password>] delete ID
    ID is the kernel ID that will be deleted

bx ae kernels [--user <user>] [--password <password>] get ID
    ID is the kernel ID whose information will be returned

bx ae kernels [--user <user>] [--password <password>] interrupt ID
    ID is the kernel ID that will be interrupted

bx ae kernels [--user <user>] [--password <password>] ls  

bx ae kernels [--user <user>] [--password <password>] restart ID
    ID is the kernel ID that will be restarted

bx ae kernels [--user <user>] [--password <password>] specs
```

## Options
Flag       | Description
---------- |  ----------------------------------------------------
--user     | A user with authority to get the version information
--password | The password for the selected user

## Sub Commands
- [create](#create)
- [delete](#delete)
- [get](#get)
- [interrupt](#interrupt)
- [ls](#ls)
- [restart](#restart)
- [specs](#specs)

### create

Creates a kernel instance.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] create SPEC_NAME
```

Args:

- `SPEC_NAME` is the name of a kernel specification.
- Use `bx ae kernels specs` for available kernel specification names.


### delete

Kills a kernel instance and removes the kernel ID.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] delete ID
```

Args:

- `ID` is the kernel ID that will be deleted.

### get

Gets information about a specific kernel instance.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] get ID
```

Args:

- `ID` is the kernel ID whose information will be returned.

### interrupt

Issues an interrupt request to the kernel instance.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] interrupt ID
```

Args:

- `ID` is the kernel ID that will be interrupted.

### ls

Lists kernel instances.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] ls
``` 

Args:

- none 

### restart

Issues a restart request to the kernel instance.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] restart ID
```
Args:

- `ID` is the kernel ID that will be restarted.

### specs

Shows the kernel specifications.

Usage:

```
bx ae kernels [--user <user>] [--password <password>] specs
```
 
Args:

- none 
