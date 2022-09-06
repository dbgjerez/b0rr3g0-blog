---
title: "GitOps and ArgoCD"
date: 2022-09-01
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
type: post
---

It's very important how we operate our ```Kubernetes or OpenShift``` cluster, in other words, how is manage the lifecycle of ours objects.
<!--more-->
Some tools like ```Helm``` are used like package manager, but with some lacks as possible rollback or applying new versions. 

Nowadays, ```GitOps``` appears as new option which try to improve our day by day working. 

# What is GitOps

GitOps ensures a unique true source like is Git, which provides a lot of features like security, versioning, distributing development, pull request, etc.

Git contains the desired state of one or some pieces on our cluster. Whereas, GitOps guarantee the synchronization between both.

In this way, Git becomes in our central piece, simplifying the different process and able to make new features. 

# Why ArgoCD

```ArgoCD``` is an ```Open Source``` and ```Cloud Native``` tool. It is responsible to syncronize the git status with the applications running on the cluster. 

ArgoCD runs a controller which is continously monitoring the applications and repositories. 

It is able to work with simple yaml files, Helm charts or Kustomize. As a result, ArgoCD deploy process always it's the same. 

![ArgoCD running](/images/argocd.png)

## Main features

# References

* https://argo-cd.readthedocs.io/en/stable/
* https://github.com/dbgjerez/kustomize-vs-helm