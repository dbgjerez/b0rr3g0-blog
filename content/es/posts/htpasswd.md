---
title: "Gestión de usuarios con HTPasswd"
date: 2022-08-17
tags: ["htpasswd", "security"]
series: ["Manage Openshift users"]
type: post
---
HTPasswd es una herramienta que nos facilita la gestión de listas de usuarios y sus credenciales.

En este artículo veremos cómo crear listas de usuarios, actualizar, etc. 
<!--more-->

# Fichero HTPasswd
Un fichero HTPasswd contiene ```n``` líneas. Cada linea representa un usuario y su contraseña, separados por el caracter ```:```.

El nombre de usuario puede ser visualizado en claro, mientras que la contraseña se encontrará encriptada.

A continuación, podemos visualizar el ejemplo de un fichero que contiene dos usuarios: ```developer``` y ```tester```. Sus contraseñas, serán ```dev12345``` y ```test12345``` respectivamente. 

```properties
developer:$apr1$ukdXlwVY$/ghTjaQageNScb0KEwsKX1
tester:$apr1$vhnjzebZ$.emX47WYDASfh4M6Ue5Wv0
```

# Cómo utilizar HTPasswd
HTPasswd ofrece muchas opciones de configuración, actualización, encriptación, etc. A continuación, podemos ver una tabla a modo de resumen de los principales comandos.

|Opción|Descripción|
|---|---|
|-c|Crea un fichero|
|-n|Solo muentra el resultado por pantalla, sin modificar el fichero|
|-b|Recibe la contraseña del usuario desde la línea de comandos (inseguro)|
|-B|Encripta la contraseña con bcrypt (recomendado)|
|-D|Elimina un usuario del fichero|
|-v|Verifica la contraseña de un usuario específico|

> Nota: Por defecto, la contraseña es encriptada haciendo uso del algoritmo MD5. Este algoritmo es muy inseguro, por tanto se recomienda utilizar la opción ```-B```. Las opciones ```-2``` o ```-5``` encriptan haciendo uso de ```SHA-256``` y ```SHA-512``` respectivamente, siendo opciones seguras. 

# Ejemplo
## Creación de un fichero
Creación de un fichero nuevo con el usuario ```developer```

```bash
$ htpasswd -Bb -c htpasswd.file developer dev12345
```

## Añadir un usuario
El siguiente ejemplo añade el usuario ```tester``` al fichero ```htpasswd.file```.
We will add a new user to the below file: 

```bash
$ htpasswd -Bb htpasswd.file tester test12345
```

## Eliminación de usuario
En este ejemplo, eliminaremos el usuario que se añadió en el momento de la creación del fichero. 

```bash
$ htpasswd -D htpasswd.file developer
```

Si vemos el contenido del fichero

```bash
$ cat htpasswd.file 
tester:$2y$05$cBIhPEkbczcmNswTIXowqODge1b5HiXn/96n55iTpuK4Z1oS2UFOG
```

# Referencias
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider
* https://httpd.apache.org/docs/2.4/programs/htpasswd.html
