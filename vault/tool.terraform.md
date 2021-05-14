---
id: 043c2a82-5512-4493-9eac-7752d9a12a7b
title: Terraform
desc: ''
updated: 1620989192035
created: 1619283599312
---

# Basic Terraform workflow 

1. Create Terraform file. Eg: `main.tf`
2. Define configuration
3. Run `terraform init`
4. Run `terraform plan` to see what Terraform wiill do
5. Run `terraform apply` to actually create the instance

# Terraform State

- Records information about what infrastructure it created in `.tfstate`.
- The state file `.tfstate` is a **Private API**
    - Should nenver be edited or referenced directly
- It should not be commited into git repo
- The best way to manage shared storage for state file is through remote backend
    - There's limitation
        - It's a 2 step process for when creating & deleting
        - `backend` block in Terraform does not allow us to use any variables or references
- Be paranoid with how we store our state files. Always enable encryption and who we allow access to it.

### Example of creating Terraform Backend
- `.tfstate` is stored in S3 and DynamoDB is used for locking
```
provider "aws" {
    region = "ap-southeast-1"
}

resource "aws_s3_bucket" "terraform_state" {
    bucket = "toy-terraform-state-fm"

    # prevent accidental deletion of this s3 bucket
    lifecycle {
        prevent_destroy = true
    }

    versioning {
        enabled = true
    }

    # enable server-side encryption by default
    server_side_encryption_configuration {
        rule {
            apply_server_side_encryption_by_default {
              sse_algorithm = "AES256"
            }
        }
    }
}

resource "aws_dynamodb_table" "terraform_locks" {
    name = "toy-terraform-state-locks-fm"
    billing_mode = "PAY_PER_REQUEST"
    hash_key = "LockID"

    attribute {
        name = "LockID"
        type = "S"
    }
}

# Example of using remote backend to avoid commiting `.tfstate`
# It solved the following issues:
# 1. manual error
# 2. locking (with Dynamo DB)
# 3. secrets (with S3 and IAM policies)

# Run `terraform init` again to instruct terraform to store state file in S3 bucket
terraform {
  backend "s3" {
      bucket = "toy-terraform-state-fm"
      key = "global/s3/terraform.tfstate"
      region = "ap-southeast-1"

      dynamodb_table = "toy-terraform-state-locks-fm"
      encrypt = true
  }
}
```