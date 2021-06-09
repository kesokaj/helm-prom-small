# helm-prom-small



````
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm -n monitoring install vmon prometheus-community/prometheus -f prom.yaml


alertmanager:
  enabled: false
server:
  enabled: true
  retention: 30d
  extraFlags:
    - web.enable-lifecycle
    - web.enable-admin-api
    - storage.tsdb.no-lockfile
    - storage.tsdb.wal-compression
    - storage.tsdb.retention.size=60GB
  global:
    scrape_interval: 5m
    scrape_timeout: 10s
    evaluation_interval: 1m
  persistentVolume:
    enabled: true
    size: 64Gi
    storageClass: cephrbd      
pushgateway:
  enabled: false
  
  
---
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: prometheus-demo-vmar-xyz
  namespace: monitoring
spec:
  entryPoints:
    - websecure
  routes:
  - match: Host(`prometheus.demo.vmar.xyz`)
    kind: Rule
    services:
    - name: vmon-prometheus-server
      port: 80
  tls:
    certResolver: default  
````
