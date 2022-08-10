---
title: "Contanedores con Podman"
date: 2022-07-10
draft: true
tags: ["docker", "container", "podman"]
series: ["Containerizing an application with Podman"]
---
Al hablar de contenedor, es común hablar de Docker. Docker ha sido durante varios años la plataforma de contenedores más usada y popular. 

En los últimos años, Podman ha ido añadiendo nuevas funcionalidades y mejorando algunas ya existentes. En este artículo veremos en detalle cómo trabajar con Podman. 

# Ventajas de usar Podman

Las principales ventajas del uso de Podman frente a Docker son el ```daemon``` principal y el uso de ```root```. 

Docker necesita de un proceso principal corriendo, mientras que podman es ```daemonless```, es decir no es necesario tener un proceso continuamente corriendo en nuestro sistema. Como resultado, cada contenedor es un hilo hijo del proceso principal. 

La otra ventaja principal de podman es el usuario. Docker necesita que cada contenedor corra como root, mientras que con Podman no es necesario, puedes elegir cómo quieres que se ejecute cada contenedor. 

Al contrario de docker, podman no posee la capacidad de orquestar contenedores del mismo modo que hace docker con Docker Swarm. 

|Feature|Docker|Podman|
|---|---|---|
|Daemon|Sí|No|
|Root|Siempre|Root o no|
|Orquetación|Sí, con swarm|No soportado|

# Comandos principales

|Comando|Descripción   |
|---|---|
|[images](#podman-images)      |Lista todas las imágenes en el registry local   |
|[search](#podman-search)      |Busca una imagen en los registries remotos   |
|[pull](#podman-pull)          |Descarga una imagen   |
|[run](#podman-run)            |Ejecuta un contenedor   |
|[ps](#podman-ps)              |Muestra la información de los contenedores locales  |
|[stop](#podman-stop)          |Para uno o varios contenedores   |
|[kill](#podman-kill)          |Mata el proceso principal de un contenedor con una señal específica  |
|[rm](#podman-rm)              |Elimina un contenedor   |
|[exec](#podman-exec)          |Ejecuta un comando dentro de un contenedor   |
|[build](#podman-build)        |Construye una imagen a partir de un fichero de instrucciones   |
|[tag](#podman-tag)            |Añade un nombre y versión a una imagen   |
|[push](#podman-push)          |Sube una imagen a un registry   |
|[login](#podman-login)        |Login en un registry  |
|[restart](#podman-restart)    |Reinicia un contenedor   |
|[rmi](#podman-rmi)            |Elimina una imagen local   |

A continuación, se van a mostrar algunos ejemplos y las opciones principales de los comandos indicados. 

## podman images
Lista todas las imágenes

```zsh
podman images
REPOSITORY                TAG         IMAGE ID      CREATED       SIZE
docker.io/library/golang  1.19        da8d5a6f7a03  33 hours ago  1.02 GB
```

## podman search
|opción|descripción|
|---|---|
|--filter|Filtro para la query|
|--format|Formatea la salida|
|--is-official|Busca solo imágenes oficiales|

En el siguiente ejemplo vamos a realizar una búsqueda de todas las imágenes que contengan la palabra ```golang```:

Aplicaremos un filtro para buscar solo imágenes oficiales: (```--filter is-official=true```).

También vamos a formatear la salida para mostrar únicamente el nombre y la descripción: ```--format "{{.Name}}\t{{.Description}}"``` 

```zsh
podman search golang --filter is-official=true --format "{{.Name}}\t{{.Description}}"
docker.io/library/golang    Go (golang) is a general purpose, higher-lev...
```

## podman pull
Este comando descarga una imagen concreta. Únicamente hay que indicar el tag de la imagen a descargar. 

En el siguiente ejemplo, vamos a descargar la versión ```1.19``` de la imagen ```golang```.

```zsh
podman pull docker.io/library/golang:1.19
```

## podman run

|Opción|Descripción|
|---|---|
|-v host:container|Monta un directorio local en el contenedor|
|-p port_host:port_container|Realiza la exposición de un puerto del contenedor a uno de la máquina host|
|-d|Modo en segundo plano|
|-e VAR=value|Añade una variable de entorno|
|-i|Modo interactivo|
|-t|Normalmente se utiliza con la opción -i. En este caso añade algunas opciones de interacción como stdin, stdout, etc|
|--name|Añade un nombre al contenedor|

En este ejemplo, vamos a desplegar un servidor http con un fichero ```index.html``` propio. 

La opción ```-v``` será necesaria para montar nuestro volumen local con los ficheros html.

Indicando ```-p``` el contenedor, y por tanto nuestro servidor HTTP será accesible desde la máquina host. 

Ejecutaremos el contenedor en segundo plano y en modo interactivo con las opciones ```-itd```

Vamos a llamar al contenedor ```http-server``` haciendo uso de la opción ```-name```.

```zsh
podman  run \
        -dit \
        --name HTTP-example \
        -p 8080:80 \
        -v "$PWD":/usr/local/apache2/htdocs/ \
        --name http-server \
        httpd:2.4
```

## podman ps

|Opción|Descripción|
|---|---|
|-a|Visualiza todos los contenedores, sea cual sea su estado|

En el siguiente ejemplo vemos todos los contenedores, concretamente veremos el servidor desplegado en el ejemplo anterior.

```zsh
podman ps -a
CONTAINER ID  IMAGE                        COMMAND           CREATED             STATUS                      PORTS                 NAMES
3274108641a5  docker.io/library/httpd:2.4  httpd-foreground  About a minute ago  Exited (137) 4 seconds ago  0.0.0.0:8080->80/tcp  http-example
```

## podman stop

|Opción|Descripción|
|---|---|
|-a|Para todos los contenedores en ejecución|

En este ejemplo vamos a parar nuestro servidor web, llamado ```http-example```, podemos indicar el nombre o el id. 

```zsh
podman stop http-example
```

## podman kill

|Opción|Descripción|
|---|---|
|-s|Especifica la señal a mandar al proceso principal del contenedor|
|-a|Envía la señal a todos los contenedores en estado de ejecución en el sistema|

En el siguiente ejemplo, vamos a mandar la señal ```TERM``` a todos los contenedores en ejecución.

```zsh
podman kill \
    --signal TERM \
    -a
```

## podman rm

|Opción|Descripción|
|---|---|
|-a|Elimina todos los contenedores|
|-f|Fuerza la eliminación del contenedor|

En este ejemplo, eliminaremos todos los contenedores en estado de no ejecución. Es importante destacar que podman no elimina contenedores en ejecución, para ello, primero tendríamos que parar el mismo. 

```zsh
podman rm -a
```

## podman exec

|Opción|Descripción|
|---|---|
|-i|Modo interactivo|
|-t|Habilita la interacción TTY con el contenedor|
|-e|Añade variables de entorno|

## podman build

|Opción|Descripción   |
|---|---|
|--build-arg argument=value|Argumento en tiempo de compilación|
|-f path|Path donde se encuentra el Containerfile|
|--force-rm|Elimina todos los contenedores intermedios|
|--layers false|Utiliza la caché en la construcción para los contenedores intermedios|
|-t name|Indica el tag de la imagen resultante|

En este caso, vamos a utilizar el fichero de imagen ```Containerfile```, haciendo uso de la opción ```-f Containerfile```

Vamos a utilizar el tag ```b0rr3g0/example-tag-image``` en la versión ```0.1```. En este caso, la opción a añadir es: ```-t b0rr3g0/example-tag-image:0.1```

```zsh
podman build \
    -t b0rr3g0/example-tag-image:0.1 \
    -f Containerfile
```

## podman tag

El uso del comando ```tag``` es sencillo, solo añade un nombre a una imagen existente. 

En este ejemplo, crearemos un nombre nuevo de imagen a partir de ```b0rr3g0/example-tag-image:0.1```, llamada ```b0rr3g0/golang-ms```.

```zsh
podman tag b0rr3g0/example-tag-image:0.1 b0rr3g0/golang-ms
```

## podman push

El comando ```push```, se utiliza para subir imágenes.

En este caso, subiré la imagen ```b0rr3g0/golang-ms```.

```zsh
podman push b0rr3g0/golang-ms
```

## podman login

|Opción|Descripción   |
|---|---|
|-u usuario|Indica el usuario|
|-p contraseña|Indica la contraseña del usuario|

Un ejemplo de uso sería el login del usuario ```dborrego``` en el registry ```quay.io``` con la contraseña ```12345```.



```zsh
podman login quay.io -u dborrego                                                
Password: 
Login Succeeded!
```

## podman restart

|Opción|Descripción   |
|---|---|
|-a|Reinicia todos los contenedores|

En el siguiente ejemplo voy a reiniciar el contenedor cuyo id comienza por ```a25...```:

```zsh
podman restart a25
```

## podman rmi

|Opción|Descripción   |
|---|---|
|-a|Elimina todas las imágenes|

El siguiente ejemplo elimina todas las imágenes locales: 

```zsh
podman rmi -a
```

# References
* https://docs.podman.io/en/latest/
* 