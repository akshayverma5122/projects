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
### otel collector deployment 

1. deploy the otel-collector using collector crd. 
```
kubectl -n opentelemetry create -f ./yaml-files/collector-ds-crd.yaml
```
2. run the golang pod and install the telemetrygen in it. 
```
kubectl run golang-alpine   --image=golang:1.25-alpine   --restart=Never   -- sleep infinity
k exec -it golang-alpine -- /bin/sh
go install github.com/open-telemetry/opentelemetry-collector-contrib/cmd/telemetrygen@latest
```
3. exec into golang pod and send the sample trace to otel collector. 
```
telemetrygen traces \
  --otlp-insecure \
  --otlp-endpoint otel-collector-collector.opentelemetry.svc.cluster.local:4317 \
  --service test-service \
  --traces 10
```


