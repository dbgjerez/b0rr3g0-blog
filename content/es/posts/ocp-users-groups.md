---
title: "Asignación de roles a usuarios y grupos en Openshift"
date: 2022-08-24
tags: ["Openshift", "htpasswd", "security"]
series: ["Manage Openshift users"]
---
La gestión de qué puede hacer quién en un cluster de Openshift o Kubernetes es una de las principales tareas para una correcta gestión de la seguridad. 

Openshift utiliza un sistema RBAC, donde una serie de roles pueden ser asignados a usuarios y grupos, habilitando ciertas posibilidades.
<!--more-->
En este artículo veremos qué es un grupo, cómo asignar usuarios a grupos y la asignación de roles.

# User
Un ```user``` es la representación lógica de una persona o entidad que puede interactuar con Openshift para solicitar determinados recursos. 

La gestión de usuarios fue descrita en el artículo anterior de la presente serie de artículos: [Configuración de HTPasswd en Openshift para gestión de usuarios]({{< ref "/posts/ocp-htpasswd.md" >}} )

# Groups
Un grupo es la representación de una serie de usuarios que comparten una misma serie de permisos. 

## Creación de un grupo de usuarios
Los grupos de usuarios se crean con el comando ```oc adm groups```. A continuación, vemos cómo crear un grupo llamado ```developers```:

```zsh
oc adm groups new developers
```

## Añadir usuarios a un grupo
Una vez creado el grupo, ya podemos añadirle usuarios. En este ejemplo, vamos a añadir el usuario ```Bob``` al grupo ```developers```.

```zsh
oc adm groups add-users developers Bob
```

# Role
Un role representa una determinada acción como ver, editar, crear proyectos, etc. Por defecto, Openshift ofrece un listado de errores.

La lista completa de roles y sus descripción, puede ser visualizada en el siguiente enlace: [default-roles_using-rbac](https://docs.openshift.com/container-platform/4.10/authentication/using-rbac.html#default-roles_using-rbac)

|Default cluster role|
|---|
|admin|
|basic-user|
|cluster-admin|
|cluster-status|
|cluster-reader|
|edit|
|self-provisioner|
|view|

Cuando asignamos un role a un usuario o grupo, se asigna permisos para realizar la acción o acciones que el role habilita. Por ejemplo, si damos el role ```view``` al grupo ```developers``` sobre el proyecto ```boxes```, estaremos dando permisos de visión a todos los usuarios de dicho grupo sobre el proyecto boxes. 

Este ejemplo se realizaría con el siguiente comando:

```zsh
oc adm policy add-role-to-group view boxes
```

En caso de querer asignar un role a un solo usuario, el comando a utilizar sería el mismo pero con la opción ```add-role-to-user```.

# References
* https://docs.openshift.com/container-platform/4.8/authentication/identity_providers/configuring-htpasswd-identity-provider.html
* https://docs.openshift.com/container-platform/4.10/authentication/using-rbac.html
* https://github.com/dbgjerez/ocp-HTPasswd-file-provider