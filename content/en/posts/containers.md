---
title: "How to create a container"
date: 2022-07-28
draft: true
tags: ["docker", "container", "podman"]
series: ["Containerizing an application with Podman"]
---

# Principal commands

|command|description   |
|---|---|
|FROM|declares the image to use as base|
|LABEL|metada|
|MAINTAINER|indicates the Author|
|RUN|executes shell commands as new layer|
|EXPOSE|indicates a port to expose|
|ENV|environment variable|
|ADD|copies files or folders from a local o remote source. It also unpacks .tar files|
|COPY|copies files or folder from the working directory|
|USER|specifies the username or UID that the container will use to run|
|ENTRYPOINT|specifies the default command when the container is running. By default it is ```/bin/sh -c```|
|CMD|indicates the default params for the ENTRYPOINT instruction|