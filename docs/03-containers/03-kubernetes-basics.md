# Kubernetes Basics

## Introduction
Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

## Why Kubernetes?
- Container orchestration
- Automatic scaling
- Self-healing
- Load balancing
- Rolling updates
- Service discovery

## Core Concepts

### Pods
- Smallest deployable units
- One or more containers
- Shared network namespace
- Shared storage
- Lifecycle management

### Services
- Stable network endpoints
- Load balancing
- Service discovery
- Internal/external access
- Session affinity

### Deployments
- Declarative updates
- Rollout management
- Rollback capability
- Scaling
- Health monitoring

## Basic Components

### Nodes
- Worker machines
- Run containers
- Managed by control plane
- Resource allocation
- Health monitoring

### Control Plane
- API Server
- Scheduler
- Controller Manager
- etcd
- Cloud Controller Manager

## Basic Commands

### Cluster Management
```bash
# Get cluster info
kubectl cluster-info

# Get nodes
kubectl get nodes

# Get pods
kubectl get pods

# Get services
kubectl get services
```

### Resource Management
```bash
# Create resource
kubectl apply -f <file>

# Delete resource
kubectl delete -f <file>

# Describe resource
kubectl describe <resource> <name>

# Get logs
kubectl logs <pod>
```

## Basic Resources

### Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

### Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

### Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## Advanced Concepts

### ConfigMaps
- Configuration data
- Environment variables
- Command-line arguments
- Configuration files
- Volume mounts

### Secrets
- Sensitive data
- Base64 encoding
- Volume mounts
- Environment variables
- Access control

### Volumes
- Persistent storage
- EmptyDir
- HostPath
- PersistentVolume
- StorageClass

## Networking

### Services Types
- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
- Headless

### Ingress
- HTTP/HTTPS routing
- Load balancing
- SSL termination
- Name-based virtual hosting
- Path-based routing

## Security

### Authentication
- Service accounts
- User accounts
- X509 certificates
- Bearer tokens
- OpenID Connect

### Authorization
- RBAC
- ABAC
- Webhook
- Node authorization
- AlwaysAllow/AlwaysDeny

## Best Practices

### Resource Management
- Resource requests
- Resource limits
- Horizontal scaling
- Vertical scaling
- Autoscaling

### Health Checks
- Liveness probes
- Readiness probes
- Startup probes
- Health endpoints
- Monitoring

### Deployment Strategies
- Rolling update
- Recreate
- Blue/Green
- Canary
- A/B testing

## Common Use Cases

### Web Application
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80
  type: LoadBalancer
```

### Database
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db
spec:
  serviceName: db
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - name: db
        image: postgres:13
        ports:
        - containerPort: 5432
        volumeMounts:
        - name: db-data
          mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
  - metadata:
      name: db-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

## Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Kubernetes Concepts](https://kubernetes.io/docs/concepts/)
- [Kubernetes API Reference](https://kubernetes.io/docs/reference/)
- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/cluster-administration/cluster-management/)
- [Kubernetes Examples](https://kubernetes.io/docs/tutorials/) 