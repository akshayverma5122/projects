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
