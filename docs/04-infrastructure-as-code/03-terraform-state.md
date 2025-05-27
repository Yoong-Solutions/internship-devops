# Terraform State Management

## Introduction
Terraform State is a JSON file that contains information about the infrastructure managed by Terraform. State management is crucial for effective Terraform usage.

## Why State Management?
- Resource tracking
- Metadata management
- Synchronization
- Performance
- Security

## State Types

### Local State
- File terraform.tfstate
- JSON format
- Manual backup
- Team sharing
- Version control

### Remote State
- S3 backend
- Azure Storage
- GCS backend
- Terraform Cloud
- Custom backend

## Backend Configuration

### S3 Backend
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "path/to/state/file"
    region         = "us-west-2"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

### Azure Storage Backend
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "terraform-state"
    storage_account_name = "tfstate"
    container_name      = "tfstate"
    key                 = "terraform.tfstate"
  }
}
```

## State Operations

### Commands
```bash
# Show state
terraform show

# List resources
terraform state list

# Move resource
terraform state mv 'aws_instance.old' 'aws_instance.new'

# Remove resource
terraform state rm 'aws_instance.example'

# Import resource
terraform import aws_instance.example i-123456
```

### State Locking
```hcl
terraform {
  backend "s3" {
    # ... other config ...
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

## State Security

### Encryption
- At-rest encryption
- In-transit encryption
- KMS integration
- Access control
- Audit logging

### Access Control
- IAM policies
- Role-based access
- Resource policies
- Network policies
- Audit trails

## State Best Practices

### Organization
- State file structure
- Environment separation
- Resource naming
- State file location
- Backup strategy

### Security
- Encryption at rest
- Encryption in transit
- Access control
- Audit logging
- Regular rotation

### Performance
- State file size
- Remote state
- State locking
- Concurrent access
- Caching

## Common Patterns

### Multi-environment
```hcl
# prod/main.tf
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
  }
}

# dev/main.tf
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "dev/terraform.tfstate"
  }
}
```

### State Separation
```hcl
# network/main.tf
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "network/terraform.tfstate"
  }
}

# app/main.tf
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "app/terraform.tfstate"
  }
}
```

## State Troubleshooting

### Common Issues
- State conflicts
- Lock issues
- Access denied
- Performance problems
- Corruption

### Debugging
- State inspection
- Lock verification
- Access testing
- Performance analysis
- Log analysis

## State Migration

### Local to Remote
```bash
# Initialize new backend
terraform init -migrate-state

# Verify migration
terraform show
```

### Remote to Remote
```bash
# Update backend config
terraform init -migrate-state -backend-config="bucket=new-bucket"
```

## Resources
- [Terraform State](https://www.terraform.io/docs/language/state)
- [State Backends](https://www.terraform.io/docs/language/settings/backends)
- [State Locking](https://www.terraform.io/docs/language/state/locking)
- [State Security](https://www.terraform.io/docs/language/state/sensitive-data)
- [State Best Practices](https://www.terraform-best-practices.com/state) 