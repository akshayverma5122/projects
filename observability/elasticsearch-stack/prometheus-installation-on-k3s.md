
1. add the prometheus helm repo and save the default configuration values. 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm pull prometheus-community/kube-prometheus-stack --version
helm show values prometheus-community/kube-prometheus-stack > prom-default-values.yaml
```
