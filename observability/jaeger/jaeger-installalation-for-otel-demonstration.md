```
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
```
```
helm install -n jaeger-system  my-jaeger jaegertracing/jaeger --version 4.8.0
```
