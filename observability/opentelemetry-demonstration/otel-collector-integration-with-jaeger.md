### otel collector deployment + collector integration with jaeger instance

1. install the jaeger instance using jaeger-installalation-for-otel-demonstration.md guided steps. for collector integration with jaeger use below code.
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
4. exec into golang pod and send the sample trace to otel collector using otel endpoint.  
```
telemetrygen traces \
  --otlp-insecure \
  --otlp-endpoint otel-collector-collector.opentelemetry.svc.cluster.local:4317 \
  --service test-service \
  --traces 10
```
5. login in the jaeger instance and see the traces under test-service

<img width="959" height="483" alt="image" src="https://github.com/user-attachments/assets/cc14382d-de92-4728-a80b-b9956b9726c9" />

