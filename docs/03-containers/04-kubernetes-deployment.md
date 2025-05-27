# Kubernetes Deployment

## Introduction
Kubernetes deployments provide a declarative way to manage application updates, scaling, and rollbacks. This guide covers advanced deployment strategies and best practices.

## Deployment Strategies

### Rolling Update
- Gradual replacement
- Zero downtime
- Health checks
- Rollback capability
- Resource management

### Recreate
- Complete replacement
- Downtime expected
- Simple process
- Resource cleanup
- State management

### Blue/Green
- Two environments
- Traffic switching
- Zero downtime
- Quick rollback
- Resource duplication

### Canary
- Gradual rollout
- Risk mitigation
- Traffic splitting
- Metrics monitoring
- Feature flags

## Deployment Configuration

### Basic Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:1.0.0
        ports:
        - containerPort: 80
```

### Advanced Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:1.0.0
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
```

## Deployment Management

### Commands
```bash
# Create deployment
kubectl apply -f deployment.yaml

# Update deployment
kubectl set image deployment/my-app my-app=my-app:1.0.1

# Rollback deployment
kubectl rollout undo deployment/my-app

# Check status
kubectl rollout status deployment/my-app

# View history
kubectl rollout history deployment/my-app
```

### Scaling
```bash
# Scale deployment
kubectl scale deployment/my-app --replicas=5

# Autoscale deployment
kubectl autoscale deployment/my-app --min=2 --max=10 --cpu-percent=80
```

## Advanced Features

### Pod Disruption Budget
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: my-app
```

### Pod Anti-Affinity
```yaml
spec:
  template:
    spec:
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - my-app
              topologyKey: kubernetes.io/hostname
```

### Resource Quotas
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 4Gi
    limits.cpu: "8"
    limits.memory: 8Gi
```

## Monitoring and Logging

### Health Checks
- Liveness probes
- Readiness probes
- Startup probes
- Custom health endpoints
- Metrics collection

### Logging
- Container logs
- Application logs
- System logs
- Audit logs
- Log aggregation

## Security

### Network Policies
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-app-network-policy
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 80
```

### Security Context
```yaml
spec:
  template:
    spec:
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 2000
      containers:
      - name: my-app
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop:
            - ALL
```

## Best Practices

### Resource Management
- Set resource requests
- Set resource limits
- Use horizontal scaling
- Monitor resource usage
- Optimize container images

### Deployment Strategy
- Choose appropriate strategy
- Implement health checks
- Configure rollback
- Monitor deployments
- Test in staging

### Security
- Use security contexts
- Implement network policies
- Manage secrets
- Regular updates
- Security scanning

## Troubleshooting

### Common Issues
- Deployment failures
- Pod scheduling
- Resource constraints
- Network issues
- Configuration errors

### Debugging
- Check pod status
- View pod logs
- Describe resources
- Check events
- Verify configurations

## Resources
- [Kubernetes Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Deployment Strategies](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#deployment-strategies)
- [Rolling Updates](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)
- [Deployment Best Practices](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#deployment-strategies)
- [Kubernetes Security](https://kubernetes.io/docs/concepts/security/) 