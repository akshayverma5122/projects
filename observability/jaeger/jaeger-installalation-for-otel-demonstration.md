```
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
```
```
kubectl create namespace jaeger-system
helm install -n jaeger-system  my-jaeger jaegertracing/jaeger --version 4.8.0
```
```
export POD_NAME=$(kubectl get pods --namespace jaeger-system -l "app.kubernetes.io/instance=my-jaeger,app.kubernetes.io/component=all-in-one" -o jsonpath="{.items[0].metadata.name}")
```
```
kubectl port-forward --namespace jaeger-system $POD_NAME 16686:16686 --address 0.0.0.0
```

