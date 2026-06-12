### otel operator deployment 

1. add the helm chart repo for otel operator 
```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
```
2. search the available version of otel operator and download the default values of helm chart. download the helm chart as well. 
```
helm search repo open-telemetry
helm show values open-telemetry/opentelemetry-operator --version=0.114.1 > otel-operator-default-values.yaml
helm pull open-telemetry/opentelemetry-operator --version=0.114.1
```

3. install the otel operator using helm and verify it. 
```
helm install otel-operator ./yaml-files/opentelemetry-operator-0.114.1.tgz \
  -n opentelemetry \
  --create-namespace \
  --values=./yaml-files/otel-operator-default-values.yaml
```
4. verify the otel operator.
```
kubectl --namespace opentelemetry get pods -l "app.kubernetes.io/instance=otel-operator"
```
