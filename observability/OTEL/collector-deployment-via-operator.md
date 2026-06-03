### otel operator deployment 

```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm search repo open-telemetry
helm show values open-telemetry/opentelemetry-operator --version=0.114.1 > otel-operator-default-values.yaml
```
```
helm -n opentelemetry install otel-operator open-telemetry/opentelemetry-operator  --create-namespace --values=otel-operator-de
fault-values.yaml 
kubectl --namespace opentelemetry get pods -l "app.kubernetes.io/instance=otel-operator"
```
### otel collector deployment 
```
kubectl create -f otel-collector.yaml -n opentelemetry
```
```
kubectl run golang-alpine   --image=golang:1.25-alpine   --restart=Never   -- sleep infinity
k exec -it golang-alpine -- /bin/sh
go install github.com/open-telemetry/opentelemetry-collector-contrib/cmd/telemetrygen@latest
```
```
telemetrygen traces \
  --otlp-insecure \
  --otlp-endpoint otel-collector-collector.opentelemetry.svc.cluster.local:4317 \
  --service test-service \
  --traces 10
```
