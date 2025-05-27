# Docker Compose

## Introduction
Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure application services, networks, and volumes.

## Why Docker Compose?
- Multi-container applications
- Development environments
- Testing environments
- Single host deployments
- Service dependencies

## Basic Concepts

### Services
- Container definitions
- Build configurations
- Environment variables
- Dependencies
- Resource limits

### Networks
- Default network
- Custom networks
- Network aliases
- Network drivers
- Network isolation

### Volumes
- Named volumes
- Bind mounts
- Volume drivers
- Volume permissions
- Data persistence

## Compose File Structure

### Basic Example
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=mydb
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Common Directives
- version: Compose file version
- services: Service definitions
- networks: Network configurations
- volumes: Volume definitions
- configs: Configuration files
- secrets: Sensitive data

## Basic Commands

### Service Management
```bash
# Start services
docker-compose up

# Start in background
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs

# Rebuild services
docker-compose build
```

### Service Operations
```bash
# List services
docker-compose ps

# Execute command
docker-compose exec <service> <command>

# Scale services
docker-compose up --scale <service>=3

# View service logs
docker-compose logs <service>
```

## Advanced Features

### Environment Variables
```yaml
services:
  web:
    environment:
      - NODE_ENV=development
      - DB_HOST=db
    env_file:
      - .env
      - .env.local
```

### Health Checks
```yaml
services:
  web:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
```

### Resource Limits
```yaml
services:
  web:
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

## Common Use Cases

### Web Application Stack
```yaml
version: '3.8'

services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - web

  web:
    build: .
    environment:
      - NODE_ENV=production
    depends_on:
      - db
      - redis

  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### Development Environment
```yaml
version: '3.8'

services:
  app:
    build: .
    volumes:
      - .:/app
      - /app/node_modules
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    command: npm run dev

  db:
    image: postgres:13
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_DB=dev
      - POSTGRES_USER=dev
      - POSTGRES_PASSWORD=dev
```

## Best Practices

### Development
- Use volume mounts
- Hot reloading
- Development tools
- Debug configurations
- Local testing

### Production
- Use specific versions
- Resource limits
- Health checks
- Logging
- Security

### Security
- Use secrets
- Limit capabilities
- Network isolation
- Regular updates
- Access control

## Troubleshooting

### Common Issues
- Port conflicts
- Volume permissions
- Network connectivity
- Resource constraints
- Service dependencies

### Debugging
- View logs
- Check status
- Inspect containers
- Test connectivity
- Verify configurations

## Resources
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Compose File Reference](https://docs.docker.com/compose/compose-file/)
- [Compose CLI Reference](https://docs.docker.com/compose/reference/)
- [Compose Best Practices](https://docs.docker.com/compose/best-practices/)
- [Compose Examples](https://docs.docker.com/compose/samples/) 