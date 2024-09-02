# EC2 CloudFormation Template

## Purpose

The `ec2.yaml` CloudFormation template is designed to automate the deployment of a load-balanced EC2 architecture in AWS. This template sets up multiple EC2 instances behind an Application Load Balancer (ALB), ensuring that incoming traffic is distributed evenly across the instances.

## Overview

- **Load Balancer**: The template provisions an Application Load Balancer (ALB) that distributes incoming HTTP traffic across the EC2 instances.

- **EC2 Instances**: Launches two EC2 instances in different public subnets, each with a web server configured to respond to HTTP requests.

- **Security Groups**: Configures security groups to allow HTTP traffic to the load balancer and between the instances.

- **HTML Pages**: Each EC2 instance hosts a simple HTML page that can be accessed via the instance's public IP address.

- **AMI ID Management**: The AMI ID for the EC2 instances is securely managed via the AWS Systems Manager Parameter Store.

## Usage

### 1. Set Up the Required Parameters

Before creating the EC2 stack, ensure that the AMI ID is stored in the AWS Systems Manager Parameter Store:

- **AMI ID**: Store the AMI ID under `/ec2/AmiId` in the Parameter Store.

<br>

### 2. To create the load-balanced EC2 environment using this template, execute the following command:

    aws cloudformation create-stack --stack-name MyEC2Stack --template-body file://cloudformation_templates/ec2.yaml

<br>

### 3. Access the Web Page

#### 3.1 After the stack is created, follow these steps to access the web page hosted on each EC2 instance:

Note the public IP addresses of the EC2 instances (WebServerInstance1A and WebServerInstance2B).

<br>

#### 3.2 Open a web browser and enter the public IP address of either instance:

    http://your-public-ip-address
    
You should see the HTML page with the following content:

    <!DOCTYPE html>
    <html>
    <head>
        <title>Welcome to S3 hosted Website</title>
    </head>
    <body>
        <h1>Welcome to S3 hosted Website</h1>
        <p>This is a static Website on S3</p>
    </body>
    </html>
<br>

### 4. To delete the EC2 instances, load balancer, and associated resources, use:
    
    aws cloudformation delete-stack --stack-name MyEC2Stack
