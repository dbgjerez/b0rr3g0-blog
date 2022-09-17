---
title: "ArgoCD components"
date: 2022-09-01
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
---

ArgoCD is one of the most powerful GitOps tools. In this article, we will discover his architecture, user management, good practices and his objects. 
<!--more-->

# ArgoCD Architecture

The following picture represents the different components of ArgoCD.

![ArgoCD running](/images/argocd-architecture.png)

* **API** 

An internal gRPC/REST server exposes the API.

* **Repository Service** 

Internal service which contains a cache of the application Git repository.

* **Application Controller** 

The application controller which is continuously monitoring running applications. 

If it detects a difference between the live state and the desired state, the application turn on ```OutOfSync``` state. 

When an application is in <<OutOfSyc>> state, the controller can invokes the ```PreSync```, ```Sync``` and ```PostSync``` hooks. This process can be auto or manual.

# ArgoCD users and privileges

# ArgoCD Cloud Native objects

Cloud Native is an adjective for an application or tool, which runs by default on the cloud, taking the most advantage of it. In this case, ArgoCD is designed to work natively on Kubernetes. 

The ArgoCD configuration can be declarative and Cloud Native. In other words, we can configure it only with native CRDs of Kubernetes. 

Thanks to this approach, we can do GitOps over our ArgoCD installation. 

## ArgoCD CRD

ArgoCD is the main CRD. To stand up an ArgoCD instance, you have to deploy it. 

When an instance is created, the ArgoCD controller creates and configures an ArgoCD stack, with every necessary piece to work fine.

The following example shows a possible instance:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ArgoCD
metadata:
  name: openshift-gitops
spec:
  rbac:
    defaultPolicy: ''
    policy: |
      g, cluster-admins, role:admin
    scopes: '[groups]'
  server: 
    route:
      enabled: true
  dex:
    openShiftOAuth: true
```

> NOTE: this example contains some commands for Openshift. If you run it in Kubernetes, you just have to omit the server route and dex. 

## AppProject

An AppProject CRD is a group of applications that defines a series of information about it:

* **sourceRepos:** a list of repositories that can use the applications within the AppProject.
* **destinations:** reference cluster and namespaces where the applications can be deployed.
* **roles:** list of roles that you need to access the resources.

This example shows an 
#TODO

## Secret
### Git repository
### Helm repository
### Clusters
## Application
## ApplicationSet

// TODO: add all different argocd objects and his explanation