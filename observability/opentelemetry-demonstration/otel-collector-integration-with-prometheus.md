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
