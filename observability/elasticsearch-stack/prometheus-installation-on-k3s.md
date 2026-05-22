
## prometheus installation 

1. add the prometheus helm repo and save the default configuration values. 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm pull prometheus-community/kube-prometheus-stack --version 85.0.3
helm show values prometheus-community/kube-prometheus-stack --version 85.0.3 > prom-default-values.yaml
```
