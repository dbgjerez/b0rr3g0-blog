---
title: "Métricas en aplicationes Go"
date: 2022-07-21
tags: ["monitoring", "prometheus", "go", "gin-gonic"]
series: ["Monitoring a Microservice"]
type: post
---

En este artículo vamos a exponer las métricas en el formato esperado por Prometheus, como base de monitorización Open Source, escalable y Cloud Native. 
<!--more-->

Para la exposición de las métricas vamos a utilizar las propias librerias de Prometheus. Es muy importante evitar lo máximo posible el acoplamiento a estas librerias, ya que si algún día queremos modificar nuestro stack de monitorización, la tarea sea lo más sencilla posible.

# Librerias 

Prometheus nos ofrece tres librerias, lo primero será añadirlas a nuestro proyecto:

```zsh
go get github.com/prometheus/client_golang/prometheus
go get github.com/prometheus/client_golang/prometheus/promauto
go get github.com/prometheus/client_golang/prometheus/promhttp
```

# Código de monitorización

En mi caso, estoy utilizando gin (Gin Framework), por tanto tengo que crear un handler para exponer las métricas:

```golang
package interfaces

import (
	"github.com/gin-gonic/gin"
	"github.com/prometheus/client_golang/prometheus/promhttp"
)

func MetricsHandlerGetHandler() func(c *gin.Context) {
	return func(c *gin.Context) {
		prometheusHandler := promhttp.Handler()
		prometheusHandler.ServeHTTP(c.Writer, c.Request)
	}
}
```

Dicho handler se añade a la ruta:

```golang
func main() {
	router := gin.Default()
	router.GET("/metrics", interfaces.MetricsHandlerGetHandler())
	router.Run(":8080")
}
```

# Consulta de métricas

Si ejecuto mi aplicación: 

```golang
❯ go run app.go
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /api/v1/health            --> golang-k8s-helm-helloworld/interfaces.HealthcheckGetHandler.func1 (3 handlers)
[GIN-debug] GET    /api/v1/grettings         --> golang-k8s-helm-helloworld/interfaces.MessageGetHandler.func1 (3 handlers)
[GIN-debug] GET    /metrics                  --> golang-k8s-helm-helloworld/interfaces.MetricsHandlerGetHandler.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

Podemos ver como está disponible el endpoint ```/metrics```

Si realizamos una consulta sobre dicho endpoint:

```zsh
❯ curl localhost:8080/metrics
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0
go_gc_duration_seconds{quantile="0.5"} 0
go_gc_duration_seconds{quantile="0.75"} 0
go_gc_duration_seconds{quantile="1"} 0
go_gc_duration_seconds_sum 0
go_gc_duration_seconds_count 0
...
```

El listado de métricas es mucho más extenso. 

Estas métricas serán leidas por Prometheus como veremos en posteriores entradas. 

# References

- https://github.com/dbgjerez/golang-k8s-helm-helloworld
- https://prometheus.io/docs/guides/go-application/
