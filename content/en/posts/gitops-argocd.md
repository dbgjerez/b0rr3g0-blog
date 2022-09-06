---
title: "GitOps and ArgoCD"
date: 2022-09-01
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
type: post
---

It's very important how we operate our Kubernetes or OpenShift cluster, in other words, how is manage the lifecycle of ours objects.

Some tools like ```Helm``` are used like package manager, but with some lacks as possible rollback or applying new versions. 

Nowadays, ```GitOps``` appears as new option which try to improve our day by day working. 

# What is GitOps

GitOps ensures a unique true source like is Git, which provides a lot of features like security, versioning, distributing development, pull request, etc.

Git contains the desired state of one or some pieces on our cluster. Whereas, GitOps guarantee the synchronization between both.

In this way, Git becomes in our central piece, simplifying the different process and able to make new features. 

#TODO# incluir diagrama

# Why ArgoCD

# References