### otel operator deployment 

```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm search repo open-telemetry
helm show values open-telemetry/opentelemetry-operator --version=0.114.1 > otel-operator-default-values.yaml
```

```
admissionWebhooks:
  certManager:
    enabled: false
```

```
kubectl create namespace opentelemetry
helm -n opentelemetry install otel-operator open-telemetry/opentelemetry-operator  --values=otel-operator-default-values.yaml
kubectl --namespace opentelemetry get pods -l "app.kubernetes.io/instance=otel-operator"
```
### otel collector deployment 

```
apiVersion: opentelemetry.io/v1beta1
kind: OpenTelemetryCollector
metadata:
  name: otel-collector
spec:
  config:
    receivers:
      otlp:
        protocols:
          grpc:
            endpoint: 0.0.0.0:4317
          http:
            endpoint: 0.0.0.0:4318
    processors:
      memory_limiter:
        check_interval: 1s
        limit_percentage: 75
        spike_limit_percentage: 15

    exporters:
      debug: {}

    service:
      pipelines:
        traces:
          receivers: [otlp]
          processors: [memory_limiter]
          exporters: [debug]
```
kubectl create -f otel-collector.yaml -n opentelemetry
```

```
