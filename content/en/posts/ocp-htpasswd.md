---
title: "How to create Openshift users with HTPasswd provider"
date: 2022-08-20
tags: ["Openshift", "htpasswd", "security"]
series: ["Manage Openshift users"]
type: post
---

It's crucial to manage our Openshift users, group and roles. This article will describe how to configure an OAuth HTPasswd provider. 
<!--more-->
Openshift allows other providers. I will describe here an easy way to create and delete users. 

# OCP OAuth server
The ```OAuth``` CRD is the way to create a new provider o modify an existing one.

You can check if it already exists an instance with the following command:

```zsh
❯ oc get OAuth                                                                   
NAME      AGE
cluster   143m
```

In the following section, we resolve how to update or create it.

## Create the secret
As we can see in the previous file, we have to indicate the type and the location of the HTPasswd file.

The first step is to create the secret, which will hold the HTPasswd file. Previously, we should have assembled the HTPasswd file, how is clarified in this post: [How to manage HTPasswd file]({{< ref "/posts/htpasswd.md" >}})

> Note: if you have a previous file, which be updated, you have to retrieve and update it with the following command: ```oc get secret htpass-secret -ojsonpath={.data.htpasswd} -n openshift-config | base64 --decode > localusers```

To create the secret, we will type the following command:

```zsh
oc create secret generic htpass-secret --from-file=htpasswd=localusers -n openshift-config
```

## Update or create the instance
The following stage is to create or update the instance. This instance has to be called ```cluster```.

The ```HTPasswd OAuth provider``` will be like this one:

```yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: localusers 
    mappingMethod: claim 
    type: HTPasswd
    htpasswd:
      fileData:
        name: htpass-secret
```

If the instance exists previously, we have to do nothing, as by updating the secret, the server will refresh the users. 

Conversely, if the instance doesn't exist, we will create it. Supposing we have a file with the previous OAuth definition called ```oauth.yaml```, we will apply it: 

```zsh
oc create -f oauth.yaml
```

# Utils
You can automate all these processes. I recommend the following link: [Script](https://github.com/dbgjerez/ocp-HTPasswd-file-provider)

In resume, this script runs the following steps: 

1. log in as an admin 
2. Reads user list
3. Retrieves existing HTPasswd file
4. Append new users to htpasswd
5. Apply the temporal file
6. Creates a new OAuth provider

# References
* https://docs.openshift.com/container-platform/4.10/authentication/identity_providers/configuring-htpasswd-identity-provider.html
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider