---
title: "How to create a container image"
date: 2022-08-30
draft: false
tags: ["docker", "container", "podman"]
series: ["Containerizing applications with Podman"]
---

Containers are a new runtime for our applications. Building a secure, fast and minimum-size image is essential. In this article, we'll do it.
<!--more-->
A container file, or Dockerfile if you are using Docker, is a file that describes how to build our container. Using the correct labels and instructions is crucial to getting a precise and optimum run.

We'll study the principal options to define a Containerfile. In addition, we'll build a Golang application and run it on a container. 

# Principal commands

A Containerfile is a series of labels which indicates how to build and run the container. The following table shows the principal labels and their descriptions. 

|command|description   |
|---|---|
|FROM|declares the image to use as base|
|LABEL|metadata|
|MAINTAINER|indicates the Author|
|RUN|executes shell commands as new layer|
|EXPOSE|indicates a port to expose|
|ENV|environment variable|
|ADD|copies files or folders from a local o remote source. It also unpacks .tar files|
|COPY|copies files or folder from the working directory|
|USER|specifies the username or UID that the container will use to run|
|ENTRYPOINT|specifies the default command when the container is running. By default it is ```/bin/sh -c```|
|CMD|indicates the default params for the ENTRYPOINT instruction|

# Application

## Build the image

The best way to understand the images is by building one. 

I have a Golang application to test and show some features. The application can be found here: [https://github.com/dbgjerez/golang-k8s-helm-helloworld](https://github.com/dbgjerez/golang-k8s-helm-helloworld)

Our objective is to run the application on a container, so we have to build the application and the container. 

It's a good practice to build on a container, as the building process would be idempotent using always the same Containerfile. 

I've used the following: 

```Docker
FROM golang:1.18-alpine

ENV APP_NAME=golang-k8s-helm-helloworld
ENV GIN_MODE=release
ENV PORT=8080
ENV WORKDIR=/go/src/app

WORKDIR $WORKDIR
COPY . .

RUN go get -d -v ./...
RUN go install -v ./...
RUN rm -rf $WORKDIR
EXPOSE $PORT

ENTRYPOINT $APP_NAME
```

In this case, I'll use the same Containerfile to build and run the application. Definitely, I'll copy the source code, build the application and subsequently delete the source code. 

To build the image, I'll use ```podman``` which is not the objective of this article, in any case, you can check how to use it here: [Podman]({{< ref "/posts/podman.md" >}})

```bash
podman build \
  -t b0rr3g0/golang-hello-world:v0.6 \
  .
```

Finally, I'll have a result which also is called a container ```image```. 

## Run the image

As we have our image on the local registry, we can run it easily. I'll run it as a daemon, mapping the port 8080 and assign the name api-test with the following command:

```bash
podman run \
  -d \
  -p 8080:8080 \
  --name api-test \
  localhost/b0rr3g0/golang-hello-world:v0.6
```

No, I'll check the response:

```bash
curl localhost:8080/api/v1/greetings
{"msg":"Hello world"}
```

And voilà... we have a portable application running.

# References

* https://github.com/dbgjerez/golang-k8s-helm-helloworld
* [Podman]({{< ref "/posts/podman.md" >}})