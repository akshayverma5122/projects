### otel collector deployment + collector integration with prometheus

1. install the jaeger instance using prometheus-installation-for-otel.md guided steps and export the metrics in prometheus format using collector.
```
    receivers:
      otlp:
        protocols:
          grpc:
            endpoint: 0.0.0.0:4317
          http:
            endpoint: 0.0.0.0:4318
      hostmetrics:
       collection_interval: 10s
       scrapers:
         cpu: {}
         memory: {}
         disk: {}
         filesystem: {}
         network: {}
    exporters:
      prometheus:
        endpoint: 0.0.0.0:8889
        namespace: otel
    service:
      pipelines:
        metrics:
          receivers: [hostmetrics,otlp]
          exporters: [prometheus]
```
2. deploy the collector using the above config.
3. test the collector if it is exporting the hostmetrics.
```
kubectl run netshoot \
  --image=nicolaka/netshoot:latest \
  --restart=Never \
  -it --rm \
  -- bash


curl  http://otel-collector-collector.opentelemetry.svc.cluster.local:8889/metrics
```
4. prometheus can scrap the collector metrics using servicemonitor and podmonitor. if collector is deployed as deployment then best to use servicemonitor and if it is deployed as daemonset then best to use podmonitor. for collector as ds, we can use both either servicemonitor or podmonitor

```
kubectl create -f service-monitor-otel-ds-collector.yaml
kubectl create -f pod-monitor-otel-ds-collector.yaml
```

5. Now, check in promthues under status > targer health. both servicemonitor and podmonitor will display the same pod as enpoint for the metrics.

<img width="635" height="344" alt="image" src="https://github.com/user-attachments/assets/b507414f-8aea-4a1d-b674-5233159bf71c" />

