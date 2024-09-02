# S3 Static Website Hosting CloudFormation Template

## Purpose

The `s3-static.yaml` CloudFormation template is designed to set up an S3 bucket configured for static website hosting. This template creates an S3 bucket, configures it to serve static content like HTML, CSS, and JavaScript files, and sets up the necessary permissions to make the content publicly accessible.

<br>

## Overview

- **S3 Bucket**: A new S3 bucket named `my-static-website-sso` is created. This bucket is configured to host a static website with `index.html` as the main entry point.
- **Website Configuration**: The bucket is set up to serve the `index.html` file as the default document when accessed.
- **Public Access**: The bucket policy allows public read access to all objects within the bucket, making the static website accessible to anyone on the internet.

<br>

## Usage

### 1. Deploy the S3 Bucket

To create the S3 bucket and configure it for static website hosting, run the following AWS CLI command:


    aws cloudformation create-stack --stack-name MyS3StaticWebsite --template-body file://cloudformation_templates/s3-static.yaml

This command will create the S3 bucket and apply the necessary configurations for hosting your static website.

<br>

### 2. Upload Your Website Files

After the bucket is created, you can upload your static website files (e.g., index.html, styles.css, app.js) to the bucket using the AWS CLI, AWS Management Console, or any S3-compatible tool.

Example using the AWS CLI:
    
    aws s3 cp cloudformation_templates/index.html s3://my-static-website-sso/

<br>

### 3. Access Your Website
Once the files are uploaded, you can access your website using the following URL format:

    http://my-static-website-sso.s3-website-region.amazonaws.com/

Replace region with the AWS region where your S3 bucket is hosted (e.g., us-west-2).

<br>

### 4. Delete the S3 Bucket
If you no longer need the static website, you can delete the S3 bucket and all its contents by deleting the CloudFormation stack:

    aws cloudformation delete-stack --stack-name MyS3StaticWebsite
