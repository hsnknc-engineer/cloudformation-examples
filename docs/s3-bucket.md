# CloudFormation S3 Bucket Setup

This CloudFormation template (`s3-bucket.yaml`) sets up a simple S3 bucket in AWS.

## Usage

### 1. Review the Bucket Name

Before deploying, make sure the bucket name specified in the `s3-bucket.yaml` file is unique across all existing S3 buckets in AWS:

### 2. Create the S3 Bucket

To create the S3 bucket, use the following AWS CLI command:
    
    aws cloudformation create-stack --stack-name MyS3BucketStack --template-body file://cloudformation_templates/s3-bucket.yaml

### 3. Delete the S3 Bucket

    aws cloudformation delete-stack --stack-name MyS3BucketStack
