# Prometheus

## Introduction
Prometheus is an open-source monitoring and alerting system designed for distributed systems and microservices.

## Why Prometheus?
- Metrics collection
- Time series storage
- Data querying
- Alerting
- Easy integration

## Core Concepts

### Metrics
- Counter
- Gauge
- Histogram
- Summary
- Custom metrics

### Time Series
- Labels
- Timestamps
- Values
- Retention
- Storage

### PromQL
- Query language
- Functions
- Operators
- Aggregations
- Time ranges

## Basic Configuration

### Prometheus Server
```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

### Service Discovery
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
```

## Monitoring Targets

### Node Exporter
```yaml
# node_exporter.yml
collectors:
  enabled:
    - cpu
    - diskstats
    - filesystem
    - loadavg
    - meminfo
    - netdev
    - netstat
    - textfile
    - time
    - vmstat
```

### Application Metrics
```python
from prometheus_client import Counter, Gauge, start_http_server

# Define metrics
http_requests_total = Counter('http_requests_total', 'Total HTTP requests')
cpu_usage = Gauge('cpu_usage', 'CPU usage percentage')

# Start server
start_http_server(8000)
```

## Alerting

### Alert Rules
```yaml
# rules.yml
groups:
  - name: example
    rules:
      - alert: HighCPUUsage
        expr: cpu_usage > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: High CPU usage
          description: CPU usage is above 80% for 5 minutes
```

### Alert Manager
```yaml
# alertmanager.yml
global:
  resolve_timeout: 5m

route:
  group_by: ['alertname']
  group_wait: 10s
  group_interval: 10s
  repeat_interval: 1h
  receiver: 'email-notifications'

receivers:
  - name: 'email-notifications'
    email_configs:
      - to: 'team@example.com'
```

## Visualization

### Grafana Dashboard
```json
{
  "dashboard": {
    "id": null,
    "title": "Node Exporter",
    "tags": [],
    "timezone": "browser",
    "panels": [
      {
        "title": "CPU Usage",
        "type": "graph",
        "datasource": "Prometheus",
        "targets": [
          {
            "expr": "100 - (avg by (instance) (irate(node_cpu_seconds_total{mode=\"idle\"}[5m])) * 100)"
          }
        ]
      }
    ]
  }
}
```

## Best Practices

### Metric Naming
- Use consistent naming
- Include units
- Use appropriate types
- Add meaningful labels
- Document metrics

### Scraping
- Configure intervals
- Use service discovery
- Handle failures
- Monitor scrape health
- Optimize targets

### Storage
- Configure retention
- Use remote storage
- Monitor disk usage
- Backup data
- Clean up old data

## Common Use Cases

### Container Monitoring
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
    metrics_path: '/metrics'
```

### Database Monitoring
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'postgres'
    static_configs:
      - targets: ['postgres-exporter:9187']
    metrics_path: '/metrics'
```

## Troubleshooting

### Common Issues
- Scrape failures
- High cardinality
- Memory usage
- Disk space
- Query performance

### Debugging
- Check targets
- Verify metrics
- Test queries
- Monitor resources
- Check logs

## Resources
- [Prometheus Documentation](https://prometheus.io/docs)
- [PromQL Reference](https://prometheus.io/docs/prometheus/latest/querying/basics)
- [Alerting Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules)
- [Service Discovery](https://prometheus.io/docs/prometheus/latest/configuration/configuration)
- [Best Practices](https://prometheus.io/docs/practices/naming) 