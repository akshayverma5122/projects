### otel operator deployment 

1. add the helm chart repo for otel operator 
```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
```
2. search the available version of otel operator and download the default values of helm chart. 
```
helm search repo open-telemetry
helm show values open-telemetry/opentelemetry-operator --version=0.114.1 > otel-operator-default-values.yaml
```

3. install the otel operator using helm and verify it. 
```
helm install otel-operator open-telemetry/opentelemetry-operator \
  -n opentelemetry \
  --create-namespace \
  -f ./yaml-files/otel-operator-default-values.yaml
```
4. verify the otel operator.
```
kubectl --namespace opentelemetry get pods -l "app.kubernetes.io/instance=otel-operator"
```
### otel collector deployment + collector integration with jaeger instance

1. for collector integration with jaeger use below code.
```
    receivers:
## collector will accept the traces, logs and metrics in otel format using this config. 
      otlp:
        protocols:
          grpc:
            endpoint: 0.0.0.0:4317
          http:
            endpoint: 0.0.0.0:4318
    exporters:
## collecto will transport the traces to jager using this config. 
      otlp_grpc/jaeger:
        endpoint: my-jaeger.jaeger-system.svc.cluster.local:4317  ## FQDN of jaeger instance
## this will skip the tls verification  
        tls:
          insecure: true
## arranging the receiver and exporter in pipeline 
    service:
      pipelines:
        traces:
          receivers: [otlp]
          exporters: [otlp_grpc/jaeger]
```

2. deploy the otel-collector using collector crd. 
```
kubectl -n opentelemetry create -f ./yaml-files/collector-ds-crd.yaml
```
3. run the golang pod and install the telemetrygen in it. 
```
kubectl run golang-alpine   --image=golang:1.25-alpine   --restart=Never   -- sleep infinity
k exec -it golang-alpine -- /bin/sh
go install github.com/open-telemetry/opentelemetry-collector-contrib/cmd/telemetrygen@latest
```
4. exec into golang pod and send the sample trace to otel collector. 
```
telemetrygen traces \
  --otlp-insecure \
  --otlp-endpoint otel-collector-collector.opentelemetry.svc.cluster.local:4317 \
  --service test-service \
  --traces 10
```


