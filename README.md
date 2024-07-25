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