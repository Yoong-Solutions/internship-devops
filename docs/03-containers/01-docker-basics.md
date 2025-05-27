# Docker Basics

## Introduction
Docker is a platform for developing, shipping, and running applications in containers. Containers are lightweight, portable, and self-contained units that can run anywhere Docker is installed.

## Why Docker?
- Consistent environments
- Isolation
- Portability
- Scalability
- Resource efficiency
- Version control and component reuse

## Core Concepts

### Images
- Read-only templates
- Built from Dockerfile
- Layered architecture
- Immutable
- Reusable

### Containers
- Running instances of images
- Isolated environments
- Ephemeral
- Resource-limited
- Network-enabled

### Dockerfile
- Build instructions
- Base image
- Dependencies
- Configuration
- Commands

## Basic Commands

### Image Management
```bash
# List images
docker images

# Pull image
docker pull <image-name>

# Build image
docker build -t <image-name> .

# Remove image
docker rmi <image-name>
```

### Container Management
```bash
# Run container
docker run <image-name>

# List containers
docker ps
docker ps -a

# Stop container
docker stop <container-id>

# Remove container
docker rm <container-id>
```

### Container Interaction
```bash
# Execute command in container
docker exec -it <container-id> <command>

# View container logs
docker logs <container-id>

# Copy files
docker cp <container-id>:<path> <local-path>
```

## Dockerfile Basics

### Basic Structure
```dockerfile
# Base image
FROM node:14

# Working directory
WORKDIR /app

# Copy files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application
COPY . .

# Expose port
EXPOSE 3000

# Start command
CMD ["npm", "start"]
```

### Common Instructions
- FROM: Base image
- WORKDIR: Working directory
- COPY: Copy files
- RUN: Execute commands
- EXPOSE: Port exposure
- CMD: Default command
- ENV: Environment variables
- ARG: Build arguments

## Best Practices

### Image Size
- Use appropriate base image
- Combine RUN commands
- Remove unnecessary files
- Use .dockerignore
- Multi-stage builds

### Security
- Use official images
- Scan for vulnerabilities
- Run as non-root
- Update regularly
- Limit capabilities

### Performance
- Use caching
- Optimize layers
- Minimize dependencies
- Use specific versions
- Clean up regularly

## Common Use Cases

### Web Application
```dockerfile
FROM nginx:alpine
COPY ./dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Database
```dockerfile
FROM postgres:13
ENV POSTGRES_DB=mydb
ENV POSTGRES_USER=user
ENV POSTGRES_PASSWORD=password
COPY ./init.sql /docker-entrypoint-initdb.d/
```

### Development Environment
```dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

## Networking

### Basic Concepts
- Bridge network
- Host network
- None network
- User-defined networks
- Network drivers

### Common Commands
```bash
# Create network
docker network create my-network

# Connect container
docker network connect my-network <container-id>

# List networks
docker network ls

# Inspect network
docker network inspect my-network
```

## Volume Management

### Types of Volumes
- Named volumes
- Bind mounts
- tmpfs mounts
- Volume drivers
- Volume plugins

### Common Commands
```bash
# Create volume
docker volume create my-volume

# List volumes
docker volume ls

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume
```

## Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Docker Security](https://docs.docker.com/engine/security/)
- [Docker Compose](https://docs.docker.com/compose/) 