---
title: "Manage users with htpasswd"
date: 2022-08-17
draft: false
tags: ["htpasswd", "security"]
series: ["Manage Openshift users"]
---

HTPasswd is a Unix tool which is used to manage elemental user lists.

At this article, we will discover how to use the  ```htpasswd``` command.
<!--more-->

# HTPasswd file

A htpasswd file contains n lines. Every line is a user and password separated by ```:```.

The password is encrypted. 

The following example shows a file that contains two users:  ```developer``` and ```tester``` with its passwords ```dev12345``` and ```test12345``` respectively.

```properties
developer:$apr1$ukdXlwVY$/ghTjaQageNScb0KEwsKX1
tester:$apr1$vhnjzebZ$.emX47WYDASfh4M6Ue5Wv0
```

# How to use
Htpasswd offers many configuration options for file generation, updating, encrypting, etc.

The following table shows the principal options:

|Option|Description|
|---|---|
|-c|Create the file|
|-n|Only show the result without modifying the file|
|-b|Reads the password from the command (unsecured)|
|-B|Encryot the password with bcrypt (very secure and recommended)|
|-D|Deletes a specific user|
|-v|Verify the password of the specified user|

> Note: If you don't use the ```-B``` option, your password will encrypt using MD5 (very insecure). You have to use at least ```-2``` or ```-5``` to use ```SHA-256``` or ```SHA-512``` encrypting algorithms.

# Example
## Create a file
Create a new file with a new user:
```bash
$ htpasswd -Bb -c htpasswd.file developer dev12345
```

## Add a new user
We will add a new user to the below file: 

```bash
$ htpasswd -Bb htpasswd.file tester test12345
```

## Delete a user
We will delete the first user added to the file: 

```bash
$ htpasswd -D htpasswd.file developer
```

If we check the result after the three commands: 

```bash
$ cat htpasswd.file 
tester:$2y$05$cBIhPEkbczcmNswTIXowqODge1b5HiXn/96n55iTpuK4Z1oS2UFOG
```

# References
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider
* https://httpd.apache.org/docs/2.4/programs/htpasswd.html
