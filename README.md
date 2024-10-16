
# WordPress Deployment CloudFormation Template Documentation

## Overview

This CloudFormation template provisions the infrastructure required for deploying a WordPress application on AWS. The stack includes EC2 instances for the web server, an RDS instance for the database, security groups, and a SNS topic for notifications.

### Resources Created

-   **EC2 Instance** for WordPress
-   **RDS (MySQL) Database**
-   **Security Groups** for web and database traffic
-   **SNS Topic** for email notifications
-   **Subnet Groups** for RDS instances

## Parameters

### General Parameters

-   **VPCStackName**: The name of the VPC stack to import networking details from.
-   **KeyName**: An existing EC2 KeyPair for SSH access.
-   **SSHLocation**: IP range that is allowed to access the EC2 instance via SSH (default is `0.0.0.0/0`).
-   **NotificationEmail**: Email address to receive notifications from the SNS topic.

### EC2 Parameters

-   **InstanceType**: Type of the EC2 instance for the web server (default is `t2.micro`).
-   **LatestAmiId**: The ID of the latest Amazon Linux AMI (default provided).

### Database Parameters

-   **DBName**: Name of the WordPress database (default is `wordpressdb`).
-   **DBUsername**: Username for the database.
-   **DBPassword**: Password for the database.
-   **DBInstanceClass**: Instance class of the RDS instance (default is `db.t3.micro`).
-   **DBAllocatedStorage**: Storage size of the database (minimum 20 GiB, maximum 100 GiB).

## Resources

### EC2 Instance (Web Server)

-   Uses the specified Amazon Linux AMI and installs WordPress along with necessary packages.
-   Includes a security group allowing inbound HTTP (port 80) and SSH (port 22) traffic.

### RDS Instance (Database)

-   A MySQL database instance with backup retention.
-   Includes a security group that restricts access to the database from the web server only.

### SNS Topic

-   Used to send notifications upon successful creation or updates of the CloudFormation stack.
-   **EmailSubscription** resource to subscribe an email address to this SNS topic.

## Outputs

-   **WebsiteURL**: The URL of the deployed WordPress site.
-   **DBEndpoint**: The endpoint address of the RDS database.
-   **SNSArn**: The Amazon Resource Name (ARN) of the SNS topic.

## How to Deploy

1.  **Upload the template to AWS CloudFormation.**
2.  **Provide the required parameters**, including the VPC stack name, EC2 KeyPair name, and the notification email.
3.  **Launch the stack**, and wait for the resources to be created.
4.  **Check your email** for a subscription confirmation from SNS. Click the link to confirm the subscription.

## Usage

-   The **SSHLocation** parameter should be set to the IP range you will use to access the EC2 instance.
-   The **KeyName** parameter must refer to a pre-existing EC2 KeyPair in your AWS account.
-   Notifications about stack events will be sent to the provided **NotificationEmail**.

## Additional Information

-   The RDS instance and EC2 instance are configured with security best practices, including restricted inbound traffic.
-   Upon deletion of the stack, RDS and Security Groups are retained or deleted based on their policies.