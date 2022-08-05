---
title: "Manage containers with podman"
date: 2022-07-28
draft: true
tags: ["docker", "container", "podman"]
series: ["Containerizing an application with Podman"]
---
lkasjdkajsd
# Principal commands

|command|description   |
|---|---|
|[images](#podman-images)      |Lists images in local registry   |
|[search](#podman-search)      |Search an image in registries   |
|[pull](#podman-pull)          |Pull an image from a registry   |
|[run](#podman-run)            |Run a container from a image   |
|[ps](#podman-ps)              |Show containers info  |
|[stop](#podman-stop)          |Stop a container   |
|[kill](#podman-kill)          |Kill a main container process with a specific signal  |
|[rm](#podman-rm)              |Delete a container   |
|[exec](#podman-exec)          |Exec a command inside the container   |
|[build](#podman-build)        |Build an image from a specific Containerfiles   |
|[tag](#podman-tag)            |Add name and version to a image   |
|[push](#podman-push)          |Push the imagen to a specific registry   |
|[login](#podman-login)        |Login into a registry   |
|[restart](#podman-restart)    |Restart a specific container   |
|[rmi](#podman-rmi)            |Delete an imagen from local   |

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

The following example find all images with golang in its name.

I will apply a filter that finds only official images (```--filter is-official=true```).

Also, the output is customized using the ```--format "{{.Name}}\t{{.Description}}"``` parameter.

```zsh
podman search golang --filter is-official=true --format "{{.Name}}\t{{.Description}}"
docker.io/library/golang	Go (golang) is a general purpose, higher-lev...
```

## podman pull
This command download the desire image, you only have to indicate the image tag. 

In this example we will download the golang image in this 1.19 version.

```zsh
podman pull docker.io/library/golang:1.19
```

## podman run

|option|description|
|---|---|
|-v host:container|Mount the local machine volume inside the pod|
|-p port_host:port_container|Expose the container pod in local machine|
|-d|Background way|
|-e VAR=value|Set environment variables|
|-i|Interactive mode|
|-t|Allocate a pseudo-TTY for container. It is often use together with -i option|
|--name|Asign a name to the container. It's very useful to manage it. |

Now I will deploy a http server with my own html code. 

I need the ```-v``` option to mount the volume with the html files. 

The ```-p``` option will be used to map the port and visualize the html files. 

It will run in interactive and background mode, so the ```-itd``` option is needed. 

```zsh
podman  run \
        -dit \
        --name http-example \
        -p 8080:80 \
        -v "$PWD":/usr/local/apache2/htdocs/ \
        httpd:2.4
```

## podman ps

|option|description|
|---|---|
|-a|Visualize all pods running or not|

The following example list all pods in the system.

```zsh
podman ps -a
CONTAINER ID  IMAGE                        COMMAND           CREATED             STATUS                      PORTS                 NAMES
3274108641a5  docker.io/library/httpd:2.4  httpd-foreground  About a minute ago  Exited (137) 4 seconds ago  0.0.0.0:8080->80/tcp  http-example
```

## podman stop

|option|description|
|---|---|
|-a|Stops all the containers|

The following example stops the pod with name ```http-example```. Anothe posibility is to pass the id instead of the name.

```zsh
podman stop http-example
```

## podman kill

|option|description|
|---|---|
|-s|Specifies a signal to send to the process|
|-a|Send signal to all running containers|

The following example sends the signal ```TERM``` to the all running containers

```zsh
podman kill \
    --signal TERM \
    -a
```

## podman rm

|option|description|
|---|---|
|-a|Removes all containers|
|-f|Force to remove the container|

The following example remove all the containers in the system.

```zsh
podman rm -a
```

## podman exec

|option|description|
|---|---|
|-i|Interactive mode|
|-t|Allocate a pseudo-TTY for container. It is often use together with -i option|
|-e|Set an environment variable|

## podman build

|option|description   |
|---|---|
|--build-arg argument=value|Argument to the builder process|
|-f path|Path to the Containerfile with the instructions to build the image|
|--force-rm|Remove all the intermediates containers|
|--layers false|Cache intermediates layers during the build. By default the value is true|
|-t name|Tagged the name of the image result|

In this case the build command depends a lot of the instructions in our ```Containerfile```.

The following example shows how to build an image with the name ```b0rr3g0/example-tag-image``` and the version ```0.1```.

On the other hand, it is using the file ```Containerfile``` to build the imagen.

```zsh
podman build \
    -t b0rr3g0/example-tag-image:0.1 \
    -f Containerfile
```

## podman tag

The ```tag``` command has not an option. In this case the command adds a name to an existing image.

This example tag the image ```b0rr3g0/example-tag-image:0.1``` with the name ```b0rr3g0/golang-ms```.

```zsh
podman tag b0rr3g0/example-tag-image:0.1 b0rr3g0/golang-ms
```

## podman push

This example push the image ```b0rr3g0/golang-ms``` to the registry.

## podman login

|option|description   |
|---|---|
|-u usuario|Indicates the user|
|-p password|Indicate the password for the user|

The follwing example login the user ```dborrego``` in the registry ```quai.io``` with the password ```12345```.

## podman restart

|option|description   |
|---|---|
|-a|Restart all the containers|

To restart a container with the id ```a25...``` we have to execute this command:

```zsh
podman restart a25
```

## podman rmi

|option|description   |
|---|---|
|-a|Remove all the images|

To clean all the local registry only have to use the following command:

```zsh
podman rmi -a
```