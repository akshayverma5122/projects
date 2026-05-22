1. Add the helm repo 
```
helm repo add elastic https://helm.elastic.co
helm repo update
```
2. View available configuration options and save it. 
```
helm show values elastic/eck-operator --version 3.0.0 > eck-operator-for-otel-demonstration.yaml
helm show values elastic/eck-elasticsearch --version 0.15.0 > eleasticsearch-for-otel-demonstration.yaml
helm show values elastic/eck-kibana --version 0.15.0 > kibana-for-otel-demonstration.yaml
```
3. download the helm charts for crds, operator, elasticsearch, kibana and logstash.  
```
helm pull elastic/eck-operator-crds  --version 3.0.0   
helm pull elastic/eck-operator --version 3.0.0
helm pull elastic/eck-elasticsearch    --version 0.15.0
helm pull elastic/eck-kibana  --version 0.15.0
```
4. install crd and operator
```
helm install  eck-crds ./eck-operator-crds-3.0.0.tgz --create-namespace --namespace=elastic-system 
helm install -n elastic-system eck-operator ./eck-operator-3.0.0.tgz --set installCRDs=false --values=eck-operator-for-otel-demonstration.yaml
```
5. install elasticsearch and kibana
```
helm install -n elastic-system elasticsearch  ./eck-elasticsearch-0.15.0.tgz --values=eleasticsearch-for-otel-demonstration.yaml
helm install -n elastic-system  kibana ./eck-kibana-0.15.0.tgz  --set elasticsearchRef.name=elasticsearch-eck-elasticsearch  --values=kibana-for-otel-demonstration.yaml
```
6. access kibana.
```
k -n elastic-system expose deployment kibana-eck-kibana-kb --type NodePort --port 5601 --target-port 56001

```
