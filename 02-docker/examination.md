# Docker and Containers - Examination

## POC Inheritance
This POC builds upon Linux basics (POC 1.1) and will be used as a foundation for:
- CI/CD pipeline containerization (POC 3.1)
- Infrastructure container management (POC 4.1)
- Kubernetes container orchestration (POC 5.1)
- Container monitoring and logging (POC 6.1)

## Project Overview: App-Glitchbox
The main project is to deploy a containerized web application (app-glitchbox) on a Linux VM with monitoring, alerting, and CI/CD integration.

### App-Glitchbox Description
App-Glitchbox is a chaos engineering application that simulates various system failures and issues. It uses ChaosBlade to create controlled chaos in the system, helping to test system resilience and monitoring capabilities.

#### Core Features
- Random CPU load spikes (90% for 60s)
- Memory pressure simulation (80% for 60s)
- Application crashes
- Network latency injection (3000ms)
- Disk space filling (512MB)
- Application hangs (10 minutes)

#### Technical Stack
- Base Image: Ubuntu 22.04
- Chaos Tool: ChaosBlade v1.7.4
- Container Management: Docker Compose
- Monitoring: Prometheus + Grafana

### System Requirements
- Linux VM (Ubuntu 22.04 LTS recommended)
- Docker Engine
- Docker Compose
- Git
- GitHub account for CI/CD
- Docker Hub account for image registry
- Minimum 2GB RAM
- 20GB free disk space

## Practice Exercises

### Exercise 1: Multi-container Web Application (App-Glitchbox)
Deploy and configure the chaos engineering application with the following components:

#### Main Application Container
- Ubuntu 22.04 base image
- ChaosBlade installation
- Custom entrypoint script
- Health check configuration
- Environment variables
- Restart policies

#### Monitoring Stack
- Prometheus container
  - Custom metrics collection
  - Alert rules configuration
  - Data retention policies
- Grafana container
  - Custom dashboards
  - Alert notifications
  - Data visualization
- Node Exporter
  - System metrics
  - Container metrics
  - Resource usage

#### Alerting System
- AlertManager configuration
- Notification channels:
  - Email
  - Slack
  - PagerDuty
- Alert rules for:
  - CPU spikes
  - Memory pressure
  - Container crashes
  - Network issues
  - Disk space

### Exercise 2: Container Monitoring System
Set up a comprehensive monitoring solution:

#### Metrics Collection
- Prometheus configuration
  - Custom metrics
  - Scrape intervals
  - Retention policies
- Node Exporter setup
  - System metrics
  - Container metrics
- Custom metrics for:
  - Chaos events
  - System health
  - Resource usage

#### Visualization
- Grafana dashboards
  - System overview
  - Chaos events
  - Resource usage
  - Alert status
- Custom panels for:
  - CPU/Memory usage
  - Network latency
  - Disk space
  - Container status

#### Alerting
- AlertManager setup
- Notification channels
- Alert rules for:
  - High CPU usage (>80%)
  - Memory pressure (>70%)
  - Container crashes
  - Network latency (>2000ms)
  - Disk space (<20%)

#### Logging
- Log aggregation
- Log rotation
- Error tracking
- Audit logging

## Assessment Criteria

### Knowledge Assessment (30%)
1. Docker concepts understanding
   - Container lifecycle
   - Image management
   - Network configuration
   - Volume management
2. Container orchestration knowledge
   - Service discovery
   - Load balancing
   - Health checks
3. Docker Compose proficiency
   - Multi-container setup
   - Environment management
   - Network configuration
4. Security best practices
   - Image security
   - Network security
   - Secret management

### Practical Assessment (40%)
1. Application deployment (10%)
   - Container configuration
   - Environment setup
   - Health checks
2. Monitoring setup (10%)
   - Prometheus configuration
   - Grafana dashboards
   - Alert rules


### Documentation Requirements (30%)
1. Architecture diagrams
   - System overview
   - Component interaction
   - Network topology
2. Configuration documentation
   - Environment setup
   - Service configuration
   - Security settings
3. Deployment procedures
   - Installation steps
   - Configuration steps
   - Verification steps
4. Troubleshooting guides
   - Common issues
   - Debug procedures
   - Recovery steps
5. Integration points
   - Previous POC integration
   - Future POC preparation

## Submission Guidelines
1. All Dockerfiles must be well-documented with:
   - Base image justification
   - Layer optimization
   - Security considerations
2. Include setup instructions:
   - Prerequisites
   - Installation steps
   - Configuration steps
3. Provide test cases:
   - Chaos scenarios
   - Recovery procedures
   - Monitoring verification
4. Document assumptions:
   - System requirements
   - Network requirements
   - Security requirements
5. Document integration points:
   - Previous POC connections
   - Future POC preparations

## Grading Rubric
- Application Deployment: 30%
  - Container configuration
  - Environment setup
  - Health monitoring
- Monitoring Setup: 20%
  - Metrics collection
  - Alert configuration
  - Dashboard design
- CI/CD Implementation: 20%
  - Pipeline automation
  - Testing coverage
  - Deployment reliability
- Documentation: 15%
  - Technical accuracy
  - Completeness
  - Clarity
- Security Implementation: 15%
  - Image security
  - Network security
  - Access control 