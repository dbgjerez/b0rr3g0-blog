---
title: "Openshift users, groups and roles"
date: 2022-08-24
tags: ["Openshift", "htpasswd", "security"]
series: ["Manage Openshift users"]
type: post
---
Once we have an Openshift cluster with a list of users, it's important to set what is possible to do for each user. 

Openshift works with an RBAC system when a list of roles can be assigned to a user or group. 
<!--more-->
In this article, we will see what a group is. How to assign users to a specific group and how to assign roles to users and groups.

# Users
A user is a person or entity that can interact with the Openshift cluster. In the previous article, we saw how to create users. 

You can see it here: [How to create OCP users]({{< ref "/posts/ocp-htpasswd.md" >}} )

# Groups
A group is a series of users that share a series of roles or permissions.

## Create an Openshift group
To create groups, we will use the command ```oc adm groups```. The following example shows how to create a group called developers.

```zsh
oc adm groups new developers
```

## Add users to the Openshift group
Once a group exists, we can add users to it. In this example, we add the user ```Bob``` to the group ```developers```.

```zsh
oc adm groups add-users developers Bob
```

# Roles
A role represents a determinate activity such as viewing, editing or creating projects. By default, Openshift offers a list of default roles.

This list of roles is the following. The description of each can be found here: [default-roles_using-rbac](https://docs.openshift.com/container-platform/4.10/authentication/using-rbac.html#default-roles_using-rbac)

|Default cluster role|
|---|
|admin|
|basic-user|
|cluster-admin|
|cluster-status|
|cluster-reader|
|edit|
|self-provisioner|
|view|

A role is assigned to a user or group and grants the privileges of doing anything. For example, the following example grants the possibility of seeing the project ```boxes``` for the group ```developers```.

```zsh
oc adm policy add-role-to-group view boxes
```

Another possibility is to add a role to a specific user. We will use the ```add-role-to-user``` option. 

# References
* https://docs.openshift.com/container-platform/4.8/authentication/identity_providers/configuring-htpasswd-identity-provider.html
* https://docs.openshift.com/container-platform/4.10/authentication/using-rbac.html
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider