# Jenkins/GitHub Actions

## Introduction
This guide covers two popular CI/CD tools: Jenkins and GitHub Actions. Both tools help automate the software development lifecycle, from building to testing and deployment.

## Jenkins

### Overview
Jenkins is an open-source automation server that helps automate the non-human part of the software development process.

### Key Features
- Distributed builds
- Plugin ecosystem
- Pipeline as code
- Extensive integration
- Customizable workflows

### Installation
```bash
# Using Docker
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts

# Using package manager (Ubuntu)
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

### Basic Pipeline
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'npm run deploy'
            }
        }
    }
}
```

### Best Practices
- Use Jenkinsfile
- Implement security
- Regular backups
- Monitor resources
- Update plugins

## GitHub Actions

### Overview
GitHub Actions is a CI/CD platform that allows you to automate your workflow directly from your GitHub repository.

### Key Features
- Native GitHub integration
- Marketplace actions
- Matrix builds
- Environment secrets
- Self-hosted runners

### Basic Workflow
```yaml
name: CI/CD Pipeline
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'
    - name: Install dependencies
      run: npm install
    - name: Run tests
      run: npm test
```

### Advanced Features

#### Matrix Builds
```yaml
jobs:
  build:
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}
```

#### Environment Secrets
```yaml
jobs:
  deploy:
    environment: production
    steps:
    - name: Deploy
      env:
        AWS_ACCESS_KEY: ${{ secrets.AWS_ACCESS_KEY }}
        AWS_SECRET_KEY: ${{ secrets.AWS_SECRET_KEY }}
      run: ./deploy.sh
```

## Comparison

### Jenkins
Pros:
- Highly customizable
- Extensive plugin ecosystem
- Self-hosted option
- Mature platform
- Free and open-source

Cons:
- Complex setup
- Resource intensive
- Manual updates
- Learning curve
- Maintenance overhead

### GitHub Actions
Pros:
- Native GitHub integration
- Easy setup
- Modern interface
- Built-in security
- Free for public repos

Cons:
- GitHub-only
- Limited customization
- Cost for private repos
- Newer platform
- Limited self-hosting

## Best Practices

### Security
- Use secrets
- Implement RBAC
- Regular audits
- Secure runners
- Dependency scanning

### Performance
- Use caching
- Optimize workflows
- Parallel jobs
- Resource limits
- Cleanup artifacts

### Maintenance
- Regular updates
- Backup configurations
- Monitor usage
- Clean old data
- Document changes

## Common Use Cases

### Node.js Application
```yaml
name: Node.js CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: actions/setup-node@v2
    - run: npm ci
    - run: npm test
    - run: npm run build
```

### Docker Application
```yaml
name: Docker CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Build Docker image
      run: docker build -t myapp .
    - name: Run tests
      run: docker run myapp npm test
```

## Resources
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Jenkins Plugins](https://plugins.jenkins.io/)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [CI/CD Best Practices](https://www.jenkins.io/doc/book/pipeline/pipeline-best-practices/) 