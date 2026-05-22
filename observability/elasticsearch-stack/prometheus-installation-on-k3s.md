
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
helm install my-kube-prometheus-stack ./kube-prometheus-stack-85.0.3.tgz --version 85.0.3 --create-namespace --namespace=prom-system --values=prom-default-values-for-otel.yaml
```
6. expose the grafana using nodeport and grab the grafana admin user password.

```
kubectl -n prom-system expose deployment my-kube-prometheus-stack-grafana \
  --type=NodePort \
  --name=grafana-service \
  --port=80 \
  --target-port=3000
```
```
kubectl --namespace prom-system get secrets my-kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```
