# Odoo17
# Deploying Odoo on AWS with High Availability

This guide describes how to deploy Odoo, an open-source ERP and CRM platform, on Amazon Web Services (AWS) with a focus on high availability and scalability.

## Architecture Overview

![Odoo AWS Architecture](architecture-diagram-url.png)

Our architecture consists of:

1. **VPC**: A Virtual Private Cloud with public and private subnets across two Availability Zones.
2. **EC2 Instances**: Running Odoo application servers in an Auto Scaling Group.
3. **RDS**: A managed PostgreSQL database for Odoo data storage.
4. **Application Load Balancer (ALB)**: Distributes incoming traffic across the Odoo instances.
5. **Security Groups**: Control inbound and outbound traffic for EC2 and RDS instances.

## Prerequisites

- An AWS account
- AWS CLI installed and configured
- Basic knowledge of AWS services and CloudFormation

## Deployment Steps

1. **Prepare the CloudFormation Template**:
   - Use the provided `odoo-stack.yaml` file.
   - Customize parameters as needed (e.g., instance types, database credentials).

2. **Launch the CloudFormation Stack**:
   for creating an AWS CloudFormation stack for deploying Odoo:

      aws cloudformation create-stack:
         This is the AWS CLI command to create a new CloudFormation stack.
         It initiates the process of provisioning resources defined in your template.
      --stack-name OdooStack:
         Specifies the name of your CloudFormation stack (in this case, “OdooStack”).
         The stack name must be unique within the AWS region.
      --template-body file://odoo-stack.yaml:
         Points to the template file (odoo-stack.yaml) that describes the resources you want to create.
         The template can be either inline (using --template-body) or hosted on Amazon S3 (using --template-url).
      --parameters:
         Allows you to pass input parameters to your stack.
         In this example, we’re providing three parameters:
            1, KeyName: The name of your EC2 key pair.
            2, DBPassword: The password for your database.
            3, OdooAdminPassword: The password for the Odoo admin user.
      --capabilities CAPABILITY_IAM:
         Specifies that the stack creation process requires the CAPABILITY_IAM capability.
         This capability allows CloudFormation to create IAM resources (like roles and policies) on your behalf.
   Remember to replace the placeholder values (YourKeyPair, YourDBPassword, and YourOdooAdminPassword) with actual values specific to your environment. Once you execute this command, CloudFormation will start creating the stack based on your template! 🚀