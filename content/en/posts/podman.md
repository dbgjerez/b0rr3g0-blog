---
title: "Manage containers with podman"
date: 2022-07-28
draft: true
tags: ["docker", "container", "podman"]
series: ["Containerizing an application with Podman"]
---

# Principal commands

|command|description   |
|---|---|
|[images](#images)      |Lists images in local registry   |
|[search](#search)      |Search an image in registries   |
|[pull](#pull)          |Pull an image from a registry   |
|[run](#run)            |Run a container from a image   |
|[ps](#ps)              |Show containers info  |
|[stop](#stop)          |Stop a container   |
|[kill](#kill)          |Kill a container with a specific signal  |
|[rm](#rm)              |Delete a container   |
|[exec](#exec)          |Exec a command inside the container   |
|[build](#build)        |Build an image from a specific Containerfiles   |
|[tag](#tag)            |Add name and version to a image   |
|[push](#push)          |Push the imagen to a specific registry   |
|[login](#login)        |Login into a registry   |
|[restart](#restart)    |Restart a specific container   |
|[rmi](#rmi)            |Delete an imagen from local   |

Now, we will see how to use each of them with complete examples

## podman images
List all the images in our local

```zsh
podman images
REPOSITORY                TAG         IMAGE ID      CREATED       SIZE
docker.io/library/golang  1.19        da8d5a6f7a03  33 hours ago  1.02 GB
```

## podman search
|option|description|
|---|---|
|--filter|Filter the query|
|--format|Format the output|

The following example find all images with golang in its name. I will apply a filter that finds only official images (```--filter is-official=true```). Also, the output is customized using the ```--format "{{.Name}}\t{{.Description}}"``` parameter.

```zsh
podman search golang --filter is-official=true --format "{{.Name}}\t{{.Description}}"
docker.io/library/golang	Go (golang) is a general purpose, higher-lev...
```

## pull

|option|example|description   |
|---|---|---|
||```podman pull golang:1.19```|Download the golang 1.19 version image |

## run

|option|example|description   |
|---|---|---|
|-v host:container|||
|-p port_host:port_container|||
|-d|||

## ps

|option|example|description   |
|---|---|---|
||||

## stop

|option|example|description   |
|---|---|---|
||||

## kill

|option|example|description   |
|---|---|---|
||||

## rm

|option|example|description   |
|---|---|---|
||||

## exec

|option|example|description   |
|---|---|---|
||||

## build

|option|example|description   |
|---|---|---|
||||

## tag

|option|example|description   |
|---|---|---|
||||

## push

|option|example|description   |
|---|---|---|
||||

## login

|option|example|description   |
|---|---|---|
||||

## restart

|option|example|description   |
|---|---|---|
||||

## rmi

|option|example|description   |
|---|---|---|
||||
