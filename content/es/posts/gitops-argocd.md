---
title: "GitOps y ArgoCD"
date: 2022-09-09
draft: false
tags: ["gitops", "argocd", "openshift", "kubernetes", "devops"]
series: ["GitOps with ArgoCD"]
type: post
---
 
Cuando trabajamos con Kubernetes u OpenShift toma vital importancia el cómo operamos nuestro clúster, es decir, cómo tratamos el ciclo de vida de nuestros objetos.
<!--more-->
Hasta ahora, algunas herramientas como ```Helm``` nos han ayudado a realizar esta tarea trabajando como gestor de paquetes. El problema es que estas herramientas tienen algunas lagunas, como son posible, rollback, fuente única de la verdad, gestión de la seguridad, etc.
 
Hoy en día aparece un nuevo concepto: ```GitOps``` el cual mejorará nuestro día a día tal y como veremos a continuación.
 
# ¿Qué es GitOps?
GitOps se encarga de asegurar Git como fuente única de la verdad, aprovechando sus principales ventajas: seguridad, versionado, desarrollo distribuido, pull requests, etc.
 
Git contiene el estado del clúster deseado de cada una de las piezas a mantener. Por otro lado, GitOps se encarga de la sincronización entre el clúster y el repositorio de codigo.
 
De este modo, el repositorio de código (Dev), quedará separa del repositorio de operación (Ops), pudiendo controlar diferentes ciclos de vida para cada uno.
 
En esta línea, Git se convierte en nuestra pieza principal de operación, simplificando los procesos de despliegue y habilitando nuestra arquitectura para nuevos horizontes de despliegues.
 
# ¿Por qué ArgoCD?
 
```ArgoCD``` es una herramienta ```Open Source``` y ```Cloud Native```. El operador de ArgoCD será el encargado de aprovisionar todo lo necesario para tener una instancia de ArgoCD funcionanado en nuestro cluster.
 
El controlador de ArgoCD estará continuamente monitorizando las aplicaciones y repositorios en busca de cambios o inconsistencias.
 
Otra ventaja de ArgoCD es en cuanto a su compatibilidad, ya que puede trabajar con ficheros simples o herramientas de aplantillado tipo Kustomize o Helm.
 
En caso de utilizar un gestor de paquetes tipo Helm, ArgoCD no utilizará el mismo para la instalación y gestión de ciclo de vida, sino que lo utilizará únicamente como motor de plantillas.
 
Este punto es muy importante, ya que hace que ArgoCD, en definitiva, siempre aplique ficheros yaml, independientemente de las herramientas utilizadas por los equipos de desarrollo o operaciones. De este modo, ArgoCD se convierte en el orquestador y catalizador del ciclo de vida de las aplicaciones.
 
![ArgoCD running](/images/argocd.png)
 
## Funciones principales
 
El listado completo de funcionalidad se pueden consultar en los enlaces de refenrecia, principalmente la documentación oficial. Al menos, creo que es importante destacar las siguientes funcionalidades:
 
* Habilidad de sincronizar más de un cluster y entorno a la vez
* Integración con diversos SSO
* Health detallado de los diferentes recursos que componen una aplicación
* Web UI
* Webhook integración
* PreSync, Sync y PostSync hooks para soportar integraciones complejas o con herramientas de terceros
 
# Referencias
 
* https://argo-cd.readthedocs.io/en/stable/
* https://github.com/dbgjerez/kustomize-vs-helm

