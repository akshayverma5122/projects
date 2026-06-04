### jaeger deployment using helm chart

1. add the helm repo for jaeger
```
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
```
2. install the jaeger. 
```
helm install my-jaeger jaegertracing/jaeger --namespace jaeger-system  --create-namespace  --version 4.8.0
```
3. expose the jaeger using nodeport.

```
kubectl -n jaeger-system expose deployment my-jaeger --type NodePort --target-port 16686 --port 16686 --name jaeger-ui
```
4. access the jaeger ui.
```
http://node-ip:node-port
```

