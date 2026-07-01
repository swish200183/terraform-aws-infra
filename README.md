# terraform-aws-infra

A Terraform project that provisions a small AWS environment — VPC, EC2, S3, and DynamoDB — using infrastructure as code, with remote state stored in S3 and locked via DynamoDB.

This is Portfolio Project 1 in a series demonstrating Terraform and AWS fundamentals, built as part of a transition into cloud infrastructure and DevOps engineering.

## Architecture

- **VPC** with a public subnet, internet gateway, and route table
- **Security group** allowing inbound SSH (22) and HTTP (80)
- **EC2 instance** (Amazon Linux 2023) running in the public subnet
- **S3 bucket** for general-purpose storage, with a random suffix to guarantee a globally unique name
- **DynamoDB table** (pay-per-request) for application data
- **Remote state** stored in S3, with state locking via DynamoDB (see `terraform-backend-bootstrap`)

## Prerequisites

- Terraform >= 1.5.0
- An AWS account with credentials configured (`aws configure`)
- The remote state backend already provisioned — see the companion [`terraform-backend-bootstrap`](../terraform-backend-bootstrap) project

## File structure

| File | Purpose |
|---|---|
| `providers.tf` | Terraform and AWS provider configuration |
| `variables.tf` | Input variables (region, CIDR blocks, instance type, etc.) |
| `main.tf` | All AWS resources: VPC, subnet, EC2, S3, DynamoDB |
| `outputs.tf` | Useful outputs after apply (VPC ID, instance IP, bucket name, table name) |
| `backend.tf` | Remote state configuration (S3 + DynamoDB lock) |

## Usage

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

To tear everything down:

```bash
terraform destroy
```

## Notes

- The security group allows SSH from `0.0.0.0/0` for simplicity in this portfolio context; in production this would be restricted to specific IP ranges.
- The AMI is looked up dynamically via a `data` source, so the instance always launches with the latest Amazon Linux 2023 image.
- Default variable values are set for quick deployment — override them with a `terraform.tfvars` file or `-var` flags as needed.

## What this project demonstrates

- Core Terraform resource and data source usage
- Variable-driven, reusable configuration
- Remote state management and state locking
- AWS networking fundamentals (VPC, subnets, routing, security groups)