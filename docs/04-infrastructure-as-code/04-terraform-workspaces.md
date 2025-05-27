# Terraform Workspaces

## Introduction
Terraform Workspaces is a feature that allows you to manage multiple environments within a single Terraform configuration. Each workspace has its own state.

## Why Workspaces?
- Multiple environments
- State separation
- Easy switching
- Code reuse
- Version management

## Workspace Management

### Commands
```bash
# List workspaces
terraform workspace list

# Create workspace
terraform workspace new dev

# Select workspace
terraform workspace select prod

# Show current workspace
terraform workspace show

# Delete workspace
terraform workspace delete dev
```

### Configuration
```hcl
# main.tf
resource "aws_instance" "example" {
  ami           = var.ami
  instance_type = var.instance_type
  
  tags = {
    Name = "example-${terraform.workspace}"
  }
}
```

## Workspace Patterns

### Environment Separation
```hcl
# variables.tf
variable "environment" {
  description = "Environment name"
  type        = string
  default     = terraform.workspace
}

variable "instance_type" {
  description = "Instance type"
  type        = string
  
  default = {
    dev  = "t2.micro"
    prod = "t2.large"
  }[terraform.workspace]
}
```

### Resource Naming
```hcl
# main.tf
locals {
  name_prefix = "my-app-${terraform.workspace}"
}

resource "aws_instance" "web" {
  tags = {
    Name = "${local.name_prefix}-web"
  }
}

resource "aws_instance" "db" {
  tags = {
    Name = "${local.name_prefix}-db"
  }
}
```

## Workspace Best Practices

### Organization
- Naming convention
- State management
- Resource tagging
- Access control
- Documentation

### Security
- Workspace isolation
- Access control
- Secret management
- Audit logging
- Compliance

### Version Control
- Git workflow
- Branch strategy
- Code review
- CI/CD integration
- Tagging

## Common Use Cases

### Development Environment
```hcl
# dev/main.tf
module "app" {
  source = "../modules/app"
  
  environment = "dev"
  instance_type = "t2.micro"
  replicas = 1
}
```

### Production Environment
```hcl
# prod/main.tf
module "app" {
  source = "../modules/app"
  
  environment = "prod"
  instance_type = "t2.large"
  replicas = 3
}
```

## Workspace Dependencies

### Module Dependencies
```hcl
# modules/app/main.tf
resource "aws_instance" "web" {
  depends_on = [aws_security_group.web]
  
  ami           = var.ami
  instance_type = var.instance_type
  vpc_security_group_ids = [aws_security_group.web.id]
  
  tags = {
    Environment = var.environment
  }
}
```

### State Dependencies
```hcl
# main.tf
data "terraform_remote_state" "network" {
  backend = "s3"
  config = {
    bucket = "my-terraform-state"
    key    = "network/${terraform.workspace}/terraform.tfstate"
  }
}
```

## Workspace Troubleshooting

### Common Issues
- State conflicts
- Workspace selection
- Resource naming
- Access control
- Configuration errors

### Debugging
- Workspace verification
- State inspection
- Resource inspection
- Access testing
- Log analysis

## Workspace Migration

### Local to Remote
```bash
# Initialize backend
terraform init -backend-config="bucket=my-terraform-state"

# Create workspaces
terraform workspace new dev
terraform workspace new prod

# Migrate state
terraform init -migrate-state
```

### Remote to Remote
```bash
# Update backend config
terraform init -migrate-state -backend-config="bucket=new-bucket"
```

## Resources
- [Terraform Workspaces](https://www.terraform.io/docs/language/state/workspaces)
- [Workspace Best Practices](https://www.terraform-best-practices.com/workspaces)
- [Workspace Patterns](https://www.terraform.io/docs/language/state/workspaces#when-to-use-workspaces)
- [Workspace Security](https://www.terraform.io/docs/language/state/workspaces#security)
- [Workspace Migration](https://www.terraform.io/docs/language/state/workspaces#migration) 