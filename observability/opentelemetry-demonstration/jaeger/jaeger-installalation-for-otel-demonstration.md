### jaeger deployment using helm chart

1. add the helm repo for jaeger
```
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
```
2. search the available helm chart for jaeger
```
helm search repo jaegertracing
```
3. downalod the helm chart and default values. 
```
helm pull jaegertracing/jaeger --version 4.8.0
helm show values jaegertracing/jaeger  > jager-default-values.yaml 
```
4. install the jaeger. 
```
helm install my-jaeger ./yaml-files/jaeger-4.8.0.tgz --namespace jaeger-system  --create-namespace  --version 4.8.0 --values=./yaml-files/jager-default-values.yaml
```
5. expose the jaeger using nodeport.
```
kubectl -n jaeger-system expose deployment my-jaeger --type NodePort --target-port 16686 --port 16686 --name jaeger-ui
```
6. access the jaeger ui.
```
http://node-ip:node-port
```





