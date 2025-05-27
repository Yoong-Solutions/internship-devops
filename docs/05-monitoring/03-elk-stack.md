# ELK Stack

## Introduction
ELK Stack is an open-source set of tools including Elasticsearch, Logstash, and Kibana, used for collecting, processing, and visualizing log data.

## Why ELK Stack?
- Centralized logging
- Real-time analysis
- Powerful search
- Data visualization
- Scalability

## Core Components

### Elasticsearch
- Distributed search engine
- Document storage
- Indexing
- Full-text search
- Analytics

### Logstash
- Log collection
- Data processing
- Filtering
- Transformation
- Output handling

### Kibana
- Data visualization
- Dashboard creation
- Search interface
- Alerting
- Reporting

## Basic Configuration

### Elasticsearch
```yaml
# elasticsearch.yml
cluster.name: my-cluster
node.name: node-1
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["node-1", "node-2"]
cluster.initial_master_nodes: ["node-1"]
```

### Logstash
```conf
# logstash.conf
input {
  beats {
    port => 5044
  }
}

filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
  date {
    match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "logs-%{+YYYY.MM.dd}"
  }
}
```

### Kibana
```yaml
# kibana.yml
server.name: kibana
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://elasticsearch:9200"]
monitoring.ui.container.elasticsearch.enabled: true
```

## Log Collection

### Filebeat
```yaml
# filebeat.yml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/*.log

output.logstash:
  hosts: ["logstash:5044"]
```

### Application Logs
```python
import logging
from pythonjsonlogger import jsonlogger

logger = logging.getLogger()
logHandler = logging.StreamHandler()
formatter = jsonlogger.JsonFormatter()
logHandler.setFormatter(formatter)
logger.addHandler(logHandler)
```

## Data Processing

### Logstash Filters
```conf
# logstash.conf
filter {
  if [type] == "apache" {
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
      match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
  }
  
  if [type] == "nginx" {
    grok {
      match => { "message" => "%{NGINXACCESS}" }
    }
  }
}
```

### Data Transformation
```conf
# logstash.conf
filter {
  mutate {
    add_field => { "environment" => "%{ENV}" }
    remove_field => [ "host" ]
  }
  
  geoip {
    source => "clientip"
    target => "geoip"
  }
}
```

## Visualization

### Kibana Dashboard
```json
{
  "dashboard": {
    "title": "Application Logs",
    "panels": [
      {
        "type": "visualization",
        "title": "HTTP Status Codes",
        "visState": {
          "type": "pie",
          "params": {
            "type": "pie",
            "addTooltip": true,
            "addLegend": true,
            "legendPosition": "right"
          }
        }
      }
    ]
  }
}
```

### Search Patterns
```json
{
  "query": {
    "bool": {
      "must": [
        { "match": { "status": "500" } },
        { "range": { "@timestamp": { "gte": "now-1h" } } }
      ]
    }
  }
}
```

## Best Practices

### Index Management
- Use index templates
- Configure lifecycle policies
- Set up aliases
- Monitor index size
- Regular maintenance

### Performance
- Optimize mappings
- Configure shards
- Use bulk operations
- Monitor resources
- Cache settings

### Security
- Enable authentication
- Configure SSL/TLS
- Set up roles
- Audit logging
- Regular updates

## Common Use Cases

### Application Logging
```json
{
  "index_patterns": ["app-logs-*"],
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "@timestamp": { "type": "date" },
      "level": { "type": "keyword" },
      "message": { "type": "text" },
      "service": { "type": "keyword" }
    }
  }
}
```

### System Monitoring
```json
{
  "index_patterns": ["system-logs-*"],
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "@timestamp": { "type": "date" },
      "host": { "type": "keyword" },
      "metric": { "type": "keyword" },
      "value": { "type": "float" }
    }
  }
}
```

## Troubleshooting

### Common Issues
- Connection problems
- Performance issues
- Data ingestion
- Search problems
- Resource constraints

### Debugging
- Check logs
- Monitor metrics
- Test connections
- Verify configurations
- Check resources

## Resources
- [Elasticsearch Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Logstash Documentation](https://www.elastic.co/guide/en/logstash/current/index.html)
- [Kibana Documentation](https://www.elastic.co/guide/en/kibana/current/index.html)
- [ELK Stack Guide](https://www.elastic.co/guide/en/elastic-stack/current/index.html)
- [Best Practices](https://www.elastic.co/guide/en/elasticsearch/reference/current/tune-for-indexing-speed.html) 