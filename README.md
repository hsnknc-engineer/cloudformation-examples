# CloudFormation Templates

This repository contains a collection of CloudFormation templates for setting up various AWS services.

## Overview

- **CloudFormation Templates**: Located in the `cloudformation_templates/` directory.

  - [VPC Template](cloudformation_templates/vpc.yaml): Sets up a VPC with subnets, route tables, and a Bastion host. 

  - [S3 Bucket Template](cloudformation_templates/s3-bucket.yaml): Creates a simple S3 bucket with a specified name.

   - [S3 Static Website Template](cloudformation_templates/s3-static.yaml): Creates an S3 bucket configured for hosting a static website, including public access permissions.

  - [IAM Template](cloudformation_templates/iam.yaml): Configures IAM users, groups, roles, and policies for access management

  - [EC2 Template](cloudformation_templates/ec2.yaml): Deploys a load-balanced setup with EC2 instances hosting simple HTML pages, accessible via their public IPs.

  - [Auto Scaling Group (ASG) Template](cloudformation_templates/asg.yaml): Automates the deployment of an Auto Scaling Group that manages EC2 instances across multiple Availability Zones and scales based on CPU utilization.

  - [RDS Template](cloudformation_templates/rds.yaml): Deploys a MySQL RDS instance with basic configuration settings suitable for testing and development.


## Usage

1. Clone this repository:

   ```bash (for example)
   git clone https://github.com/hsnknc-engineer/cloudformation-examples.git
   cd cloudformation-examples

2. Follow the specific instructions in each linked guide for deploying the respective CloudFormation stack.

   - [S3 Bucket Template - Documentation](docs/s3-bucket.md)

   - [S3 Static Website Template - Documentation](docs/s3-static.md)
   
   - [VPC Template - Documentation](docs/vpc.md)

   - [IAM Template - Documentation](docs/iam.md)

   - [EC2 Template - Documentation](docs/ec2.md)

   - [Auto Scaling Group (ASG) Template](docs/asg.md)

   - [RDS Template](docs/rds.md)