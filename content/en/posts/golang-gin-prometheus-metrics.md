---
title: "Golang application metrics"
date: 2022-07-21
draft: true
tags: ["monitoring", "prometheus", "golang", "gin-gonic"]
series: ["Monitoring a Microservice"]
---

We will see how to expose metrics in Prometheus original format as Open Source monitoring tool, scalable and Cloud Native. 
<!--more-->
Prometheus give us specific libraries for each programming language. 

As an essential point, we should avoid coupling our applications with intrusive libraries, as in the future our monitoring stack could change. 

# Libraries 

Firstly, we will add the three Prometheus libraries to our project: 

```zsh
go get github.com/prometheus/client_golang/prometheus
go get github.com/prometheus/client_golang/prometheus/promauto
go get github.com/prometheus/client_golang/prometheus/promhttp
```

# Monitoring code

As I'm using Gin-gonic as a development Framework, I have to create a specific handle to expose the metrics: 

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

Add the handler to the router: 

```golang
func main() {
    router := gin.Default()
    router.GET("/metrics", interfaces.MetricsHandlerGetHandler())
    router.Run(":8080")
}
```

# Testing the metrics

Run the application:

```golang
❯ go run app.go
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /api/v1/health            --> golang-k8s-helm-helloworld/interfaces.HealthcheckGetHandler.func1 (3 handlers)
[GIN-debug] GET    /api/v1/grettings         --> golang-k8s-helm-helloworld/interfaces.MessageGetHandler.func1 (3 handlers)
[GIN-debug] GET    /metrics                  --> golang-k8s-helm-helloworld/interfaces.MetricsHandlerGetHandler.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

The application expose the ```/metrics```` endpoint, as you can see at logs.

If we call this endpoint: 

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

We can see a lot of parameters. In the following articles, we will see how Prometheus reads these metrics and store them. 

# References

- https://github.com/dbgjerez/golang-k8s-helm-helloworld
- https://prometheus.io/docs/guides/go-application/
