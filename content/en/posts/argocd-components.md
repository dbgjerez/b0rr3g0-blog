---
title: "ArgoCD components"
date: 2022-09-01
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
---

ArgoCD is one of the most powerfull GitOps tool. In this article, we will discover his architecture, user management, good practices and his objects. 
<!--more-->

# ArgoCD Architecture

The following picture represents the different componets of ArgoCD.

![ArgoCD running](/images/argocd-architecture.png)

* **API** 

An internal gRPC/REST server which expose the API.

* **Repository Service** 

Internal service which contains a cache of the application Git repository.

* **Application Controller** 

The application controller which is continously monitoring running applications. 

If it detects a difference betweeen the live stante and the desire state, the application turn on ```OutOfSync``` state. 

When an application is in <<OutOfSyc>> state, the controller can invokes the ```PreSync```, ```Sync``` and ```PostSync``` hooks. This process can be auto or manual.

# ArgoCD users and privileges

# ArgoCD Cloud Native objects

## Application
## ApplicationSet

// TODO: add all different argocd objects and his explanation