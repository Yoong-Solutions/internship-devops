# Grafana

## Introduction
Grafana is an open-source platform for visualizing and analyzing metrics data, allowing users to create dashboards and alerts for various monitoring systems.

## Why Grafana?
- Data visualization
- Dashboard creation
- Alerting
- Multiple data sources
- High customization

## Core Concepts

### Data Sources
- Prometheus
- InfluxDB
- Elasticsearch
- Graphite
- CloudWatch

### Dashboards
- Panels
- Variables
- Templates
- Annotations
- Time ranges

### Alerting
- Alert rules
- Notifications
- Thresholds
- Conditions
- Channels

## Basic Configuration

### Grafana Server
```ini
# grafana.ini
[server]
http_port = 3000
domain = localhost

[auth.anonymous]
enabled = true
org_role = Viewer

[security]
admin_user = admin
admin_password = admin
```

### Data Source
```yaml
# datasources.yaml
apiVersion: 1

datasources:
  - name: Prometheus
    type: prometheus
    url: http://prometheus:9090
    access: proxy
    isDefault: true
```

## Dashboard Creation

### Basic Dashboard
```json
{
  "dashboard": {
    "id": null,
    "title": "System Overview",
    "tags": ["system"],
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

### Variables
```json
{
  "templating": {
    "list": [
      {
        "name": "instance",
        "type": "query",
        "datasource": "Prometheus",
        "query": "label_values(node_cpu_seconds_total, instance)"
      }
    ]
  }
}
```

## Alerting

### Alert Rules
```json
{
  "alert": {
    "name": "High CPU Usage",
    "conditions": [
      {
        "evaluator": {
          "params": [80],
          "type": "gt"
        },
        "operator": {
          "type": "and"
        },
        "query": {
          "params": ["A", "5m", "now"]
        },
        "reducer": {
          "params": [],
          "type": "avg"
        },
        "type": "query"
      }
    ],
    "executionErrorState": "alerting",
    "frequency": "1m",
    "handler": 1,
    "message": "CPU usage is above 80%",
    "noDataState": "no_data",
    "notifications": []
  }
}
```

### Notification Channels
```json
{
  "name": "Email Alerts",
  "type": "email",
  "isDefault": true,
  "settings": {
    "addresses": "team@example.com"
  }
}
```

## Best Practices

### Dashboard Design
- Use consistent layouts
- Add descriptions
- Use appropriate visualizations
- Include time ranges
- Add annotations

### Performance
- Optimize queries
- Use caching
- Limit data points
- Use appropriate intervals
- Monitor resources

### Security
- Configure authentication
- Set up authorization
- Use HTTPS
- Manage API keys
- Regular updates

## Common Use Cases

### System Monitoring
```json
{
  "dashboard": {
    "panels": [
      {
        "title": "CPU Usage",
        "type": "graph",
        "targets": [
          {
            "expr": "100 - (avg by (instance) (irate(node_cpu_seconds_total{mode=\"idle\"}[5m])) * 100)"
          }
        ]
      },
      {
        "title": "Memory Usage",
        "type": "graph",
        "targets": [
          {
            "expr": "node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes"
          }
        ]
      }
    ]
  }
}
```

### Application Monitoring
```json
{
  "dashboard": {
    "panels": [
      {
        "title": "HTTP Requests",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(http_requests_total[5m])"
          }
        ]
      },
      {
        "title": "Response Time",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(http_request_duration_seconds_sum[5m]) / rate(http_request_duration_seconds_count[5m])"
          }
        ]
      }
    ]
  }
}
```

## Troubleshooting

### Common Issues
- Data source connection
- Query performance
- Alert delivery
- Dashboard loading
- Authentication

### Debugging
- Check logs
- Verify connections
- Test queries
- Monitor resources
- Check permissions

## Resources
- [Grafana Documentation](https://grafana.com/docs)
- [Dashboard Examples](https://grafana.com/grafana/dashboards)
- [Alerting Guide](https://grafana.com/docs/grafana/latest/alerting)
- [Data Sources](https://grafana.com/docs/grafana/latest/datasources)
- [Best Practices](https://grafana.com/docs/grafana/latest/best-practices) 