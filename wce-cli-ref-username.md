---

copyright:
  years: 2017
lastupdated: "2017-07-13"

---

<!-- Attribute definitions -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# username
## Description

Sets the default username for IBM Analytics Engine commands.

## Usage

```
bx ae username
   Displays current username configuration

bx ae username --unset
   Removes username information

bx ae username USERNAME
   Sets the username to the provided value

USERNAME is the name of the user to authenticate with, e.g., clsadmin
```

## Options

Flag       | Description
---------- | ----------------------------------------------------
--user     | A user with authority to get the version information
--password | The password for the selected user
--unset    | Remove username

## Examples

### Setting username

```
$ bx ae username clsadmin
Registering user 'clsadmin'...
OK
Default user name 'clsadmin' set.
```

### Viewing username

```
bx ae username
OK

Username:   clsadmin
```

### Erasing username

```
$ bx ae username --unset
OK
Username has been erased.
```
