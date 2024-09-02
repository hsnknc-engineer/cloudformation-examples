# CloudFormation VPC Setup

This CloudFormation template (`vpc.yaml`) set up a basic Virtual Private Cloud (VPC) architecture in AWS.
<br><br>

## Overview

The `vpc.yaml` template creates the following resources:

- **VPC**: A Virtual Private Cloud with a CIDR block of `172.16.0.0/16`.
- **Subnets**: Six subnets across two Availability Zones (public and private subnets in each AZ).
- **Internet Gateway**: Provides outbound internet access for public subnets.
- **Route Table**: Routes public subnet traffic through the Internet Gateway.
- **EC2 Instances**:
  - **Bastion Host**: An instance in the public subnet for SSH access, secured via a Security Group that allows SSH only from a specified IP range.
  - **Application Instances**: Instances in private subnets, accessible via the Bastion Host.
- **Security Groups**: Protect instances by controlling inbound traffic.
<br><br>
## Key Features

- **AMI ID and IP Address Management**: The AMI ID for EC2 instances and the IP address for SSH access to the Bastion Host are securely managed via AWS Systems Manager Parameter Store. This keeps sensitive information out of the CloudFormation template.
- **Scalable Design**: The architecture is designed to be easily extendable with additional AWS resources.
<br><br>

## Architecture Diagram

The following diagram illustrates the architecture deployed by this CloudFormation template:

![image](https://github.com/user-attachments/assets/6f57a424-bd8d-4d77-9745-3ef622665813)


- The **Bastion Host** allows secure SSH access to the EC2 instances in the private App subnets.
- Public subnets are exposed to the internet via the Internet Gateway, while the App and Database subnets remain isolated.
- EC2 instances in the App subnets can communicate with instances in the Database subnets, typically for application-to-database connections.

<br>

# Usage

## 1. Navigate to the Project Directory

Ensure that you are in the root directory of the cloned repository. If you haven't cloned the repository yet, follow the instructions in the main `README.md`.

<br>

## 2. Set up the required parameters in AWS Systems Manager Parameter Store:

- **AMI ID**: Store the AMI ID under `/myapp/AmiId`.
- **Bastion IP**: Store the IP address or CIDR block for SSH access under `/myapp/BastionIp`.

Alternatively, you can specify these values directly in the `vpc.yaml` file by replacing the parameter references with the actual values.

<br>

## 3. Create the VPC

Once the parameters are set up, you can create the VPC using the following AWS CLI command:

    aws cloudformation create-stack --stack-name MyVPCStack --template-body file://cloudformation_templates/vpc.yaml

<br>

## 4. Connect to the Bastion Host

To access your VPC resources, you can connect to the Bastion Host via SSH. Here's how:

<br>

### 4.1 Generate a `.pem` File (Key Pair)

To connect to your EC2 instance (Bastion Host) via SSH, you need a `.pem` file (key pair). If you don't already have a key pair, you can create one in the AWS Management Console:

- Go to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/).
- In the left-hand menu, click on **Key Pairs** under **Network & Security**.
- Click on the **Create Key Pair** button.
- Name your key pair (e.g., `bastion`) and choose **PEM** as the file format.
- Download the `.pem` file and save it in a secure location on your local machine.

<br>


### 4.2 Set Up the `.pem` File

Once you have your `.pem` file (e.g., `bastion.pem`), you need to secure it and prepare it for SSH access:

- Move the `.pem` file to a directory of your choice on your local machine.
- Set the correct permissions for the file:

   ```bash
   chmod 400 /path/to/bastion.pem

<br>

### 4.3 Find the Public IP of the Bastion Host

To connect to the Bastion Host, you need its public IP address. You can find this in the AWS Management Console:
- Go to the EC2 Dashboard.
- In the left-hand menu, click on Instances.
- Find your Bastion Host instance in the list and note its Public IP address.

<br>

### 4.4 Transfer the `.pem` File to the Bastion Host

To securely connect from the Bastion Host to your App1 EC2 instance, you need to transfer the .pem file to the Bastion Host instance:

From your local machine, use the following scp command to copy the .pem file to the Bastion Host:

    scp -i bastion.pem bastion.pem ec2-user@your-public-ip-address:~/

Replace your-public-ip-address with the actual public IP address of the Bastion Host.

<br>

### 4.5 Connect to the Bastion Host via SSH

Now that you have the .pem file and the public IP address of the Bastion Host, you can connect to the instance:

- Open your terminal.
- Navigate to the directory where your .pem file is located
- Use the following SSH command to connect to the Bastion Host:
    ssh -i bastion.pem ec2-user@your-public-ip-address
- Replace your-public-ip-address with the actual public IP address of the Bastion Host.
- If the connection is successful, you will be logged into your EC2 instance as the ec2-user.

<br>

### 4.6 Connect to the App1 EC2 Instance from the Bastion Host

Once you are connected to the Bastion Host and have transferred the .pem file, you can use it to connect to the App1 EC2 instance:

Use the following SSH command from the Bastion Host to connect to the App1 instance:

    ssh -i bastion.pem ec2-user@private-ip-address-app1

Replace private-ip-address-app1 with the actual private IP address of the App1 EC2 instance.

<br>

### 4.7 Ping the App2 EC2 Instance from App1

From the App1 EC2 instance, you can test connectivity to the App2 instance by pinging its private IP address:

    ping private-ip-address-app2

Replace private-ip-address-app2 with the actual private IP address of the App2 EC2 instance.

<br>



## 5. Delete the VPC

If you need to delete the VPC and all associated resources, use the following AWS CLI command:
    
    aws cloudformation delete-stack --stack-name MyVPCStack


<br>

## 6. Clean Up Parameters in AWS Systems Manager (Optional)

If you no longer need the parameters in AWS Systems Manager Parameter Store, you can delete them using the following AWS CLI commands:

    aws ssm delete-parameter --name "/myapp/AmiId"
    aws ssm delete-parameter --name "/myapp/BastionIp"
