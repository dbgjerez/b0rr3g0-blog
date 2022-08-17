---
title: "Manejo de usuarios con HTPasswd"
date: 2022-08-17
draft: false
tags: ["htpasswd", "security"]
series: ["Manage Openshift users"]
---

HTPasswd es una herramienta unix para la gestión básica de listas de usuarios. 

En este artículo vamos a ver cómo trabajar con el comando ```htpasswd``` para gestionar nuestras propias listas de usuario.
<!--more-->

# Estructura fichero HTPasswd

Un fichero de htpasswd es un fichero con n línea, cada línea representa un usuario y su respectiva contraseña. 

La contraseña del usuario se encuentra encriptada. 

En el siguiente ejemplo podemos ver un fichero con dos usuarios llamados ```developer``` y ```tester``` con contraseña ```dev12345``` y ```test12345```.

```properties
developer:$apr1$ukdXlwVY$/ghTjaQageNScb0KEwsKX1
tester:$apr1$vhnjzebZ$.emX47WYDASfh4M6Ue5Wv0
```

# Uso
Htpasswd ofrece muchas opciones de configuración, encriptado, generación de ficheros, actualización, etc. 

A continuación se muestra una tabla con las principales opciones. 

|Opción|Descripción|
|---|---|
|-c|Crea el fichero|
|-n|No actualiza el fichero, sólo muentra el resultado en pantalla|
|-b|Recibe la contraseña en el propio comando (inseguro)|
|-B|Encripta la contraseña con bcrypt (muy seguro y recomendado)|
|-D|Elimina un usuario específico|
|-v|Verifica la contraseña de un usuario específico|

> Nota: La no inclusión de -B realizará un encriptado en MD5 (muy inseguro), como mínimo es recomendable utilizar las opciones ```-2``` o ```-5``` para encriptar haciendo uso de ```SHA-256``` o ```SHA-512``` respectivamente. 

# Ejemplos
## Crear un nuevo fichero
Creación de un nuevo fichero con un nuevo usuario: 

```bash
$ htpasswd -Bb -c htpasswd.file developer dev12345
```

## Añadir un nuevo usuario
Al fichero anterior, vamos a añadir un nuevo usuario:

```bash
$ htpasswd -Bb htpasswd.file tester test12345
```

## Eliminar un usuario
Si queremos eliminar el primer usuario creado: 

```bash
$ htpasswd -D htpasswd.file developer
```

Si vemos el resultado del fichero tras los tres comandos:

```bash
$ cat htpasswd.file 
tester:$2y$05$cBIhPEkbczcmNswTIXowqODge1b5HiXn/96n55iTpuK4Z1oS2UFOG
```

# Referencias
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider
* https://httpd.apache.org/docs/2.4/programs/htpasswd.html
