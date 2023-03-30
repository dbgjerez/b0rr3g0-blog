---
title: "Contenerización de aplicaciones"
date: 2022-09-05
tags: ["docker", "container", "podman", "dockerfile"]
series: ["Containerizing applications with Podman"]
type: post
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

## Construir la imagen

La mejor forma de entender el proceso de creación de una imagen es construyendo una. 

Normalmente, utilizo una aplicación, desarrollada en Go con GinGonic que expone un "Hello world". Esta aplicación la utilizo solo a modo de test, sin entrar en el código. La aplicación se puede encontrar en el siguiente enlace: [https://github.com/dbgjerez/golang-k8s-helm-helloworld](https://github.com/dbgjerez/golang-k8s-helm-helloworld)

Nuestro objetivo final es ejecutar dicha aplicación en un contenedor, así que tenemos que construirla y construir el contenedor. 

Es una buena práctica utilizar un contenedor para construir nuestras aplicaciones, de este modo aseguramos que el entorno de construcción siempre será el mismo y consecuentemente, nuestras construcciones serán idempotentes. 

En este caso, utilizaré la misma imagen para construir, como para ejecutar la aplicación: 

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

Como se puede ver los pasos que sigo son los siguientes:  
* Copio el código fuente
* Construyo la aplicación
* Elimino el código fuente

De esta forma, con un mismo proceso tengo mi imagen disponible y sin los ficheros fuentes. 

Para la construcción y ejecución, hago uso de ```podman```, el cuál no es objetivo de este artículo. Puedes ver cómo utilizarlo y sus principales comandos en el siguiente enlace: [Podman]({{< ref "/podman.md" >}})

```bash
podman build \
  -t b0rr3g0/golang-hello-world:v0.6 \
  .
```

Finalmente, una vez finalizado el proceso de construcción tenemos nuestra imagen. 

## Ejecutar la imagen

Al finalizar la construcción, nuestra imagen quedará en nuestro registry local. Para ejecutar la imagen tenemos varias opciones, en mi caso voy a exponer el puerto ````8080````, ejecutarla como un ```demonio``` y darle a mi contenedor el nombre ```api-test```:

```bash
podman run \
  -d \
  -p 8080:8080 \
  --name api-test \
  localhost/b0rr3g0/golang-hello-world:v0.6
```

Para comprobar el correcto funcionamiento del contenedor, haremos una petición a la aplicación: 

```bash
curl localhost:8080/api/v1/greetings
{"msg":"Hello world"}
```

Y... listo, ya tenemos un contenedor portable con nuestra aplicación ejecutándose. 

# References

* https://github.com/dbgjerez/golang-k8s-helm-helloworld
* [Podman]({{< ref "/podman.md" >}})