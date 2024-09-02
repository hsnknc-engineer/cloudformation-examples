# RDS Instance CloudFormation Template

## Purpose

The `rds.yaml` CloudFormation template is designed to create a simple Amazon RDS (Relational Database Service) instance. This template sets up a MySQL database with basic configurations suitable for development or testing purposes.

<br>

## Overview

- **RDS Instance**: The template creates an Amazon RDS instance with the following properties:

  - **DBInstanceIdentifier**: The name of the RDS instance is set to `MyNewRDS`.

  - **MasterUsername**: The username for the master user is set to `admin`.
  - **MasterUserPassword**: The password for the master user is set to `password`. **Note**: This is hard-coded in the template for simplicity, but in a production environment, it is recommended to use AWS Secrets Manager or AWS Key Management Service (KMS) to manage secrets.
  - **DBInstanceClass**: The instance type is set to `db.t3.micro`, which is suitable for small workloads and is eligible for the AWS free tier.
  - **Engine**: The database engine used is MySQL, version `8.0.35`.
  - **AllocatedStorage**: The storage allocated for the database is `20` GB.
  - **BackupRetentionPeriod**: Automated backups are retained for `7` days.

<br>

## Usage

### 1. Deploy the RDS Instance

To create the RDS instance using the `rds.yaml` CloudFormation template, run the following AWS CLI command:

    aws cloudformation create-stack --stack-name MyRDSStack --template-body file://cloudformation_templates/rds.yaml

<br>

### 2. Verify the Deployment
After deploying the stack, you can verify the RDS instance creation through the AWS Management Console:

Navigate to the RDS Dashboard.
Check under Databases to ensure that the MyNewRDS instance is running.
Verify that the instance properties (engine type, version, storage, etc.) match those specified in the template.

<br>


### 3. Clean Up
If you need to delete the RDS instance and all associated resources, use the following command:

    aws cloudformation delete-stack --stack-name MyRDSStack

<br>

## Notes

- Security Considerations: The master password is hard-coded in the template, which is not a best practice. Consider using AWS Secrets Manager or AWS KMS for managing credentials in a production environment.

- Cost Management: Be aware of the potential costs associated with running an RDS instance, even if it's a small instance type like db.t3.micro.

