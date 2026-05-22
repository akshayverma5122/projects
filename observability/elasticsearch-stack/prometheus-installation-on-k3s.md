
## prometheus+Grafana installation for otel demonstration

1. add the prometheus helm repo and save the default configuration values. 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm pull prometheus-community/kube-prometheus-stack --version 85.0.3
helm show values prometheus-community/kube-prometheus-stack --version 85.0.3 > prom-default-values-for-otel.yaml
```
2. Disable the extra component - alertmanager, kubeStateMetrics, nodeExporter.
   
4. Disable the extra servicemonitor - kubeProxy, kubeScheduler, kubeEtcd, kubeDns, coreDns, kubeControllerManager, kubelet, kubeApiServer
   
5. install the prometheus and grafana.
   
```
helm install my-kube-prometheus-stack prometheus-community/kube-prometheus-stack --version 85.0.3 --values=prom-default-values-for-otel.yaml
```
