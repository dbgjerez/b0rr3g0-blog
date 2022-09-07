---
title: "GitOps and ArgoCD"
date: 2022-09-07
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
type: post
---

It's crucial how we operate our ```Kubernetes or OpenShift``` cluster, in other words, how is managing the lifecycle of our objects.
<!--more-->
Some tools like ```Helm``` are used like a package manager, but with some lacks as possible rollback or new versions apply. 

Nowadays, ```GitOps``` appears as a new option that tries to improve our daily work. 

# What is GitOps

GitOps ensures a unique right source like Git, which provides many features like security, versioning, distributing development, pull requests, etc.

Git contains the desired state of one or some pieces on our cluster. Whereas, GitOps guarantee synchronization between both.

In this way, Git becomes our central piece, simplifying the different processes and able to make new features. 

# Why ArgoCD

```ArgoCD``` is an ```Open Source``` and ```Cloud Native``` tool. It's responsible of the syncronization between git status and the applications running on the cluster. 

ArgoCD runs a controller which is continuously monitoring the applications and repositories. 

ArgoCD can work with simple yaml files, Helm charts or Kustomize. ArgoCD invokes these tools to obtain the final yaml files that it applies.

This point gets particular importance, as ArgoCD will become our orchestrator and catalyser, regardless of the tools used by the developer team. 

![ArgoCD running](/images/argocd.png)

## Main features

You can check the complete list of features in the official ArgoCD documentation. At this point, I'm only going to highlight the main of them:

* Ability to manage multiple clusters.
* SSO integration
* Health status of each application resource
* Web UI
* Webhook integration
* PreSync, Sync and PostSync hooks to support complex integrations or applications rollouts.

# References

* https://argo-cd.readthedocs.io/en/stable/
* https://github.com/dbgjerez/kustomize-vs-helm