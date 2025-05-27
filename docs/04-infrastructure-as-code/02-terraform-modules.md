# Terraform Modules

## Introduction
Terraform Modules are reusable, self-contained packages of Terraform configurations that manage a specific set of resources.

## Why Modules?
- Code reuse
- Better organization
- Easy maintenance
- Consistency
- Code sharing

## Module Structure

### Basic Module
```
module/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

### Root Module
```
.
├── main.tf
├── variables.tf
├── outputs.tf
└── modules/
    └── my-module/
```

## Module Components

### Main Configuration
```hcl
# modules/ec2/main.tf
resource "aws_instance" "this" {
  ami           = var.ami
  instance_type = var.instance_type
  
  tags = {
    Name = var.name
  }
}
```

### Variables
```hcl
# modules/ec2/variables.tf
variable "ami" {
  description = "AMI ID for the EC2 instance"
  type        = string
}

variable "instance_type" {
  description = "Instance type for the EC2 instance"
  type        = string
  default     = "t2.micro"
}

variable "name" {
  description = "Name tag for the EC2 instance"
  type        = string
}
```

### Outputs
```hcl
# modules/ec2/outputs.tf
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.this.id
}

output "public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.this.public_ip
}
```

## Module Usage

### Local Modules
```hcl
# main.tf
module "web_server" {
  source = "./modules/ec2"
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  name          = "web-server"
}
```

### Remote Modules
```hcl
# main.tf
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  
  name = "my-vpc"
  cidr = "10.0.0.0/16"
  
  azs             = ["us-west-2a", "us-west-2b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]
}
```

## Module Best Practices

### Versioning
- Semantic versioning
- Version constraints
- Module registry
- Release process
- Changelog

### Documentation
- README file
- Input variables
- Output values
- Usage examples
- Requirements

### Testing
- Unit tests
- Integration tests
- Test fixtures
- Test coverage
- CI/CD integration

## Common Module Patterns

### Network Module
```hcl
# modules/network/main.tf
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  
  name = var.vpc_name
  cidr = var.vpc_cidr
  
  azs             = var.availability_zones
  private_subnets = var.private_subnets
  public_subnets  = var.public_subnets
}

module "security_groups" {
  source = "./security_groups"
  
  vpc_id = module.vpc.vpc_id
  name   = var.security_group_name
}
```

### Application Module
```hcl
# modules/app/main.tf
module "ec2" {
  source = "./ec2"
  
  ami           = var.ami
  instance_type = var.instance_type
  subnet_id     = var.subnet_id
  vpc_id        = var.vpc_id
}

module "rds" {
  source = "./rds"
  
  identifier     = var.db_identifier
  engine         = var.db_engine
  engine_version = var.db_engine_version
  instance_class = var.db_instance_class
  vpc_id         = var.vpc_id
  subnet_ids     = var.subnet_ids
}
```

## Module Composition

### Root Module
```hcl
# main.tf
module "network" {
  source = "./modules/network"
  
  vpc_name = "my-vpc"
  vpc_cidr = "10.0.0.0/16"
}

module "app" {
  source = "./modules/app"
  
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = module.network.private_subnet_ids[0]
  vpc_id        = module.network.vpc_id
}
```

## Module Dependencies

### Explicit Dependencies
```hcl
# modules/app/main.tf
resource "aws_instance" "web" {
  depends_on = [aws_security_group.web]
  
  ami           = var.ami
  instance_type = var.instance_type
  vpc_security_group_ids = [aws_security_group.web.id]
}
```

### Implicit Dependencies
```hcl
# modules/app/main.tf
resource "aws_instance" "web" {
  ami           = var.ami
  instance_type = var.instance_type
  subnet_id     = var.subnet_id  # Implicit dependency on subnet
}
```

## Troubleshooting

### Common Issues
- Module version conflicts
- Variable type mismatches
- Output value errors
- State conflicts
- Provider configuration

### Debugging
- Module source verification
- Variable validation
- Output inspection
- State analysis
- Provider logs

## Resources
- [Terraform Modules](https://www.terraform.io/docs/language/modules)
- [Module Registry](https://registry.terraform.io)
- [Module Best Practices](https://www.terraform-best-practices.com/modules)
- [Module Composition](https://www.terraform.io/docs/language/modules/develop/composition)
- [Module Testing](https://www.terraform.io/docs/language/modules/testing) 