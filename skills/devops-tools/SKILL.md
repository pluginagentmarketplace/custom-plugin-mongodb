---
name: devops-tools
description: Learn DevOps practices, containerization, orchestration, and infrastructure automation. Master Docker, Kubernetes, Terraform, and CI/CD pipelines. Use when deploying applications, setting up infrastructure, or automating workflows.
---

# DevOps Tools Skill

Master DevOps practices with comprehensive guidance on containerization, orchestration, and infrastructure automation.

## Quick Start

### Docker Basics
```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
```

### Docker Compose
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:pass@db:5432/myapp
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=myapp
    volumes:
      - pg_data:/var/lib/postgresql/data

volumes:
  pg_data:
```

### Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: myapp:latest
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url

---
apiVersion: v1
kind: Service
metadata:
  name: app-service
spec:
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 3000
  type: LoadBalancer
```

### Terraform (Infrastructure-as-Code)
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  key_name      = aws_key_pair.deployer.key_name

  tags = {
    Name = "app-server"
  }
}

output "server_public_ip" {
  value = aws_instance.app_server.public_ip
}
```

### GitHub Actions CI/CD
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm ci

    - name: Run tests
      run: npm test

    - name: Build application
      run: npm run build

    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: myregistry/myapp:latest
```

## Advanced Topics

### Kubernetes Concepts
- Pods, Deployments, Services
- ConfigMaps and Secrets
- Persistent Volumes
- Health checks and resource limits
- Network policies and RBAC

### CI/CD Pipeline Design
- Build automation
- Automated testing
- Artifact management
- Deployment strategies (rolling, blue-green, canary)
- Monitoring and rollback

### Infrastructure Automation
- IaC best practices
- State management
- Modularization and reusability
- Version control for infrastructure

### Cloud Platforms
- AWS: EC2, S3, RDS, Lambda, ECS
- Google Cloud: Compute Engine, Cloud Storage, Cloud SQL
- Azure: VMs, App Service, Cosmos DB

## Resources

- [Docker Docs](https://docs.docker.com)
- [Kubernetes Docs](https://kubernetes.io/docs)
- [Terraform Docs](https://www.terraform.io/docs)
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [AWS Documentation](https://docs.aws.amazon.com)
