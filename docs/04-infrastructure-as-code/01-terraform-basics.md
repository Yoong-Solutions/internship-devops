# Terraform Basics

## Introduction
Terraform is an open-source infrastructure as code tool that enables you to safely and predictably create, change, and improve infrastructure.

## Why Terraform?
- Infrastructure as Code
- Multi-cloud support
- State management
- Resource dependency
- Version control

## Core Concepts

### Providers
- Cloud providers (AWS, Azure, GCP)
- Infrastructure providers
- Service providers
- Custom providers
- Provider configuration

### Resources
- Infrastructure components
- Resource types
- Resource attributes
- Resource dependencies
- Resource lifecycle

### State
- Current state
- Desired state
- State file
- State locking
- Remote state

## Basic Structure

### Main Configuration
```hcl
# Configure AWS Provider
provider "aws" {
  region = "us-west-2"
}

# Create EC2 instance
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

### Variables
```hcl
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-west-2"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}
```

### Outputs
```hcl
output "instance_ip" {
  description = "Public IP of the instance"
  value       = aws_instance.example.public_ip
}
```

## Basic Commands

### Initialization
```bash
# Initialize Terraform
terraform init

# Initialize with backend
terraform init -backend-config="bucket=my-terraform-state"
```

### Planning
```bash
# Create execution plan
terraform plan

# Plan with variables
terraform plan -var="instance_type=t3.micro"
```

### Applying
```bash
# Apply changes
terraform apply

# Apply with auto-approve
terraform apply -auto-approve
```

### Destroying
```bash
# Destroy infrastructure
terraform destroy

# Destroy specific resource
terraform destroy -target=aws_instance.example
```

## State Management

### Local State
- Default storage
- JSON format
- Manual backup
- Team sharing
- Version control

### Remote State
- S3 backend
- Azure Storage
- GCS backend
- State locking
- State encryption

## Best Practices

### Code Organization
- Module structure
- Variable files
- Output files
- State files
- Documentation

### Security
- Secret management
- Access control
- Encryption
- Audit logging
- Compliance

### Version Control
- Git workflow
- Branch strategy
- Code review
- CI/CD integration
- Tagging

## Common Use Cases

### Web Application
```hcl
# VPC Configuration
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  
  name = "my-vpc"
  cidr = "10.0.0.0/16"
  
  azs             = ["us-west-2a", "us-west-2b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]
}

# EC2 Instance
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = module.vpc.public_subnets[0]
  
  vpc_security_group_ids = [aws_security_group.web.id]
  
  tags = {
    Name = "web-server"
  }
}
```

### Database
```hcl
# RDS Instance
resource "aws_db_instance" "example" {
  identifier        = "example-db"
  engine            = "mysql"
  engine_version    = "5.7"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  
  db_name  = "exampledb"
  username = "admin"
  password = "password"
  
  vpc_security_group_ids = [aws_security_group.db.id]
  db_subnet_group_name   = aws_db_subnet_group.example.name
}
```

## Troubleshooting

### Common Issues
- State conflicts
- Provider errors
- Resource dependencies
- Variable issues
- Backend configuration

### Debugging
- Log levels
- Error messages
- State inspection
- Plan analysis
- Provider documentation

## Resources
- [Terraform Documentation](https://www.terraform.io/docs)
- [Terraform Registry](https://registry.terraform.io)
- [Terraform Best Practices](https://www.terraform-best-practices.com)
- [Terraform Modules](https://www.terraform.io/docs/language/modules)
- [Terraform Providers](https://www.terraform.io/docs/language/providers) 