---
title: "Configuración de HTPasswd en Openshift para gestión de usuarios"
date: 2022-08-20
tags: ["Openshift", "htpasswd", "security"]
series: ["Manage Openshift users"]
---
Una de las principales operaciones cuando tenemos una instalación de Openshift es la correcta gestión de usuarios, roles y sus permisos. 

Este artículo profundizará en la gestión de los usuarios, haciendo uso del uno de los proveedores de indetidades que permite Openshift; HTPasswd.
<!--more-->


# OCP OAuth 
El CRD ```OAuth``` es la forma en la que Openshift nos facilita la creación de proveedores de identidad. 

En este artículo crearemos el proveedor a través de este tipo de descriptor. 

Para comprobar si ya existe una instancia de OAuth en nuestro sistema utilizaremos el siguiente comando: 

```zsh
❯ oc get OAuth                                                                   
NAME      AGE
cluster   143m
```

Como podemos ver, ya existe una instancia. En la siguiente sección veremos como crear una o actualizar una existente. 

## Creación del secreto de configuración
Uno de los atributos a indicar en el descriptor de OAuth, es el fichero de configuración con los correspondientes usuarios. 

Nuestra primera acción será la creación de dicho fichero, con los usuarios y contraseñas deseadas. Estas acciones fueron descritas en el artículo anterior de esta entrega: [Gestión de usuarios con HTPasswd]({{< ref "/posts/htpasswd.md" >}})

> Nota: si ya existía una instancia anterior, necesitamos recuperar el fichero de usuarios para posteriormente actualizarlo. De lo contrario, podríamos eliminar sin querer los usuarios existentes. Para la recuperación de dicho fichero de configuración tulizaremos el siguiente comando: ```oc get secret htpass-secret -ojsonpath={.data.htpasswd} -n openshift-config | base64 --decode > localusers```

Con el siguiente comando crearemos el secreto llamado ```htpass-secret```, el cual contendrá el fichero HTPasswd ```localusers```. 

```zsh
oc create secret generic htpass-secret --from-file=htpasswd=localusers -n openshift-config
```

## Creación o actualización de la instancia
Una vez creado nuestro secreto, el siguiente paso será crear la instancia de OAuth que leerá dicha configuración. 

La instancia se llamará ```cluster``` y su configuración será la siguiente: 

```yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: localusers 
    mappingMethod: claim 
    type: HTPasswd
    htpasswd:
      fileData:
        name: htpass-secret
```

La creación de la instancia la realizaremos con el comando ```oc create -f``` o ```oc apply -f```.

```zsh
oc create -f oauth.yaml
```

> Nota: En caso de que la instancia ya existiése, no tendremos que hacer nada más a parte de actualizar el secreto. Una vez actualizado, el operador reiniciará los ```pods```, aplicando la nueva configuración. 

# Utilidad
El proceso de creación de usuario puede ser automantizado facilmente. En el siguiente repositorio se facilita un script que realizada dicha operación: [Script](https://github.com/dbgjerez/ocp-HTPasswd-file-provider)

El script anterior realizará las siguientes operaciones:

1. Logueado en OCP como admin
2. Lectura una fichero de propiedad con los usuarios
3. Recuperación de fichero existente de usuarios
4. Actualización del fichero, añadiendo los nuevos usuarios
5. Creación del nuevo secreto
6. Actualización del servido OAuth

# Referencias
* https://docs.openshift.com/container-platform/4.10/authentication/identity_providers/configuring-htpasswd-identity-provider.html
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider