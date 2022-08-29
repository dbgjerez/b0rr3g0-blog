---
title: "Entornos virtuales en Python"
date: 2022-07-08
draft: false
tags: ["python", "virtualenv"]
type: post
---

Cuando trabajamos con Python, normalmente utilizamos una serie de módulos como apoyo. 

Cada una de estas dependencias o módulos se utiliza en una determinada versión, desacoplar estas versiones es muy importante puesto que puede modificar el comportamiento de nuestras aplicaciones. 
<!--more--> 
Los entornos virtuales de Python nos ayudan en dicha tarea.

Requisitos:
* Python
* Pip

# Entornos virtuales

La creación de un entorno virtual se realiza haciendo uso del comando ```virtualenv```.

En caso de no tener instalado "virtualenv", lo instalamos:

```zsh
❯ pip install virtualenv
```

Creación de un entorno virtual, llamado ```venv```

```zsh
❯ python -m venv venv
```

Para utilizar el entorno virtual creado:

```zsh
❯ source venv/bin/activate
```

Si listamos las dependencias o módulos instalados: 

```zsh
❯ pip list
Package    Version
---------- -------
pip        21.2.3
setuptools 57.4.0
```

# Fichero de dependencias

El fichero de dependencias ```requeriments.txt``` posee el listado de módulos necesarios para nuestra aplicación. 

## Creación de fichero de requerimientos

Partiendo de nuestro entorno virtual iniciado, vamos a instalar una libreria de forma manual, para este ejemplo voy a utilizar la librería ```pandas```.

```zsh
❯ pip install pandas
```

Si ahora listamos las librerias, veremos que se ha añadido la libreria pandas y sus dependencias:

```zsh
❯ pip list
Package         Version
--------------- -------
numpy           1.23.0
pandas          1.4.3
pip             21.2.3
python-dateutil 2.8.2
pytz            2022.1
setuptools      57.4.0
six             1.16.0
```

Una vez tengamos claras las dependencias que forman nuestro entorno, usaremos el comando ```freeze```:

```zsh
❯ pip freeze > requirements.txt

❯ cat requirements.txt 
numpy==1.23.0
pandas==1.4.3
python-dateutil==2.8.2
pytz==2022.1
six==1.16.0
```

## Instalación de las depedencias

La instalación de dependencias se realizará de forma automática, haciendo uso del comando ```pip install```.

```zsh
❯ pip install -r requirements.txt
```

# Desactivar el entorno virtual

La desactivación del entorno virtual se realiza haciendo uso del comando ```deactivate```.

```zsh
❯ deactivate 
```