---
title: "Python application configuration"
date: 2022-07-13
draft: true
tags: ["python", "cloud-native", "twelve-factor"]
---

En las aplicaciones actuales, existe un gran número de parámetros de configuración. Estos parámetros, en ocasiones dependen del entorno de ejecución. 

Para que nuestras aplicaciones sean más mantenibles, es necesario externalizar la configuración, de forma que un mismo entregable pueda ejecutarse en diferentes entornos. Podemos ampliar información [aquí](https://12factor.net/config)
<!--more-->
Aunque no es el objetivo de este artículo, el concepto "cloud-native" también es un punto a tener en cuenta, de forma que nuestras aplicaciones puedan ser fácilmente ejecutadas y configuradas en entornos cloud. 

A continuación vamos a ver cómo **externalizar la configuración en Python de forma "cloud-native"**. 

> Además de la formas detalladas a continuación, existen proveedores de secretos que nos gestionan el ciclo de vida de los mismos. Estos formarían parte del cómo se proveen y debería ser independiente de cómo la aplicación los lee.

# Variables de entornos

Las variables de entornos son una forma básica de externalizar la configuración.

Para la lectura de variables de entorno utilizaremos directamente la libreria ```os```, propia de Python.

```python
import os

user = os.getenv('USER')
print("[USER]:", user)
```

Si ejecutamos dicho fichero: 

```zsh
❯ python tmp.py
[USER]: db
```

# Ficheros de configuración

Otra opción es el uso de ficheros de configuración. Esta opción tiene la ventaja de poder manejar de forma más sencilla gran número de variables y ficheros. 

Para cargar desde ficheros externos, haremos uso de la libreria ```python-dotenv```. Normalmente cargaremos un fichero de propiedades con formato (K,V), aunque también es posible cargar ficheros de tipo json o yaml. 

El primer paso será instalar la libreria:

```zsh
pip install python-dotenv
```

Una vez instalada, simplemente tendremos que cargar la configuración: 

```python
from dotenv import load_dotenv
import os

load_dotenv(".env", override=True)

user = os.getenv('USER')
print("[USER]:", user)
```

Parámetros de la función ```load_dotenv```:
* Fichero: indicamos que nuestras varibles de entornos se encuentran en el fichero ```.env```
* Override: con esta propiedad indicamos si queremos sobreescribir las variables del sistema, dando prioridad a las de nuestro fichero de configuración.

Creamos el fichero ```.env```:

```zsh
❯ echo 'USER=dborrego' > .env
```

Ejecutamos nuestro fichero de prueba:

```zsh
❯ python tmp.py
[USER]: dborrego
```

# Referencias

* The twelve-factor app: https://12factor.net/
* https://pypi.org/project/python-dotenv/