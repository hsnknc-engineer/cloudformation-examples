# Auto Scaling Group (ASG) CloudFormation Template

## Purpose

The `asg.yaml` CloudFormation template is designed to automate the deployment of an Auto Scaling Group (ASG) in AWS. This setup ensures that a specified number of EC2 instances are always running across multiple Availability Zones (AZs) and can automatically scale based on CPU utilization.

<br>

## Overview

- **Virtual Private Cloud (VPC)**: A VPC is created as the network environment for the ASG. It includes multiple public subnets across two Availability Zones (AZ1 and AZ2).
  
- **Subnets**:
  - **Public Subnet 1A**: Located in AZ1, used for hosting EC2 instances.
  - **Public Subnet 2B**: Located in AZ2, also used for hosting EC2 instances.

- **Security Group**:
  - **Web Server Security Group**: Allows inbound HTTP traffic (port 80) from any IP address to ensure that the web servers are accessible to the public.

- **Launch Configuration**:
  - **AMI ID**: Retrieved from the AWS Systems Manager Parameter Store, which is used to launch the EC2 instances.
  - **Instance Type**: EC2 instances are of type `t2.micro`.
  - **User Data**: A script to install Apache (`httpd`) and deploy a simple HTML page on the EC2 instances.

- **Auto Scaling Group**:
  - **Minimum Size**: 1 instance.
  - **Desired Capacity**: 2 instances.
  - **Maximum Size**: 3 instances.
  - **VPCZoneIdentifier**: Specifies the subnets where the instances will be launched.

- **CloudWatch Alarm**:
  - **High CPU Utilization**: Monitors the average CPU usage across the instances. If CPU usage exceeds 70%, a scaling action is triggered.

- **Scaling Policy**:
  - **Scale Out Policy**: Increases the number of instances in the Auto Scaling Group by 1 when the High CPU Alarm is triggered.

<br>

## Architecture Diagram

![2024-08-29_22h29_27](https://github.com/user-attachments/assets/ed585b63-9e5f-4eaa-9a74-c27e90294fed)

<br>

## Usage

### 1. Deploy the Auto Scaling Group

To deploy the Auto Scaling Group and associated resources, run the following AWS CLI command:


    aws cloudformation create-stack --stack-name MyAutoScalingGroupStack --template-body file://cloudformation_templates/asg.yaml
    
<br>

### 2. Verify the Deployment
After deployment, you can verify the following:

EC2 Instances: Ensure that two EC2 instances are running across the specified subnets in different Availability Zones.
Load Balancing: The instances are automatically scaled based on CPU utilization.
Public Access: You should be able to access the default Apache web page via the public IP address of any EC2 instance.

<br>

### 3. Clean Up
To delete the Auto Scaling Group and associated resources, use the following command:

    aws cloudformation delete-stack --stack-name MyAutoScalingGroupStack

<br>

## Conclusion

This asg.yaml template provides a robust foundation for deploying a scalable, highly available application on AWS. By automatically adjusting the number of EC2 instances in response to load, it ensures that your application can handle varying levels of traffic without manual intervention.
