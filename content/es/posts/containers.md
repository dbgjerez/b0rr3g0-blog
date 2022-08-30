---
title: "Contenerización de aplicaciones"
date: 2022-09-01
draft: false
tags: ["docker", "container", "podman", "dockerfile"]
series: ["Containerizing applications with Podman"]
---

Los contenedores son una forma de ejecutar nuestras aplicaciones. Construir contenedores seguros, rápidos y de tamaño pequeño es esencial en nuestras arquitecturas. En este artículo veremos cómo hacerlo.
<!--more-->
Un ```Containerfile``` o ```Dockerfile```, no es más que un fichero de testo con las etiquetas adecuadas para indicar cómo tiene que ser construido y ejecutado nuestro contenedor. Del correcto uso de dichas etiquetas o instrucciones, dependerá la correcta ejecución y optimización de nuestras aplicaciones. 

En este artículo mostraré las principales opciones a la hora de construir contenedores. Además, utilizaremos una aplicación de ejemplo, programada en Golang, la cuál compilaremos y ejecutaremos sobre un contenedor. 

# Comandos principales

A continuación muestro una tabla con las principales instrucciones a indicar en un fichero de contenedor y una breve descripción indicando para qué se usa. 

Cabe recordar que cada línea de nuestro fichero de contenedor, será una instrucción a ejectar, y consecuentemente, una capa del mismo. 

|Comando|Descripción   |
|---|---|
|FROM|declara la imagen a partir de la cuál generar nuestro contenedor|
|LABEL|metadata|
|MAINTAINER|indica el autor|
|RUN|ejecuta comandos shell|
|EXPOSE|indica los puertos que expone el contenedor|
|ENV|variables de entorno|
|ADD|copia ficheros o carpetas desde el host al contenedor. Además, descomprime ficheros ```.tar```|
|COPY|copia ficheros desde el host al contenedor|
|USER|especifíca el usuario o UID que ejecuta la aplicación del contenedor|
|ENTRYPOINT|especifíca el comando por defecto de entrada en el contenedor. Por defecto es: ```/bin/sh -c```|
|CMD|indica los parámetros que recibirá por defecto el ENTRYPOINT|

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