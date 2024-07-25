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
3. **Monitor Stack Creation**:
- Use AWS Console or CLI to monitor the stack creation process.
- This may take 15-20 minutes to complete.

4. **Access Odoo**:
- Once the stack is created, find the ALB DNS name in the Outputs section.
- Access Odoo using this DNS name in your web browser.

5. **Initialize Odoo**:
- Follow the Odoo setup wizard to create your first database.

## Key Components

### VPC and Networking
- Two public subnets in different Availability Zones for high availability.
- Internet Gateway for public internet access.

### EC2 and Auto Scaling
- Launch Template defines the Odoo instance configuration.
- Auto Scaling Group ensures at least two instances are running.

### RDS Database
- Managed PostgreSQL instance for data persistence.
- Multi-AZ deployment for high availability.

### Application Load Balancer
- Distributes traffic across Odoo instances.
- Performs health checks to ensure traffic is sent only to healthy instances.

### Security
- Security groups control traffic to EC2 and RDS instances.
- RDS instance is not publicly accessible.

## Customization and Scaling

- Adjust the Auto Scaling group's minimum, maximum, and desired capacity as needed.
- Modify the Launch Template to change instance types or add custom configurations.
- Scale the RDS instance vertically as your database grows.

## Cleanup

To remove all created resources:

1. Go to the CloudFormation console in AWS.
2. Select the stack you created.
3. Click "Delete" and confirm.
4. Wait for all resources to be deleted.

## Troubleshooting

- If instances are unhealthy, check the Odoo logs: `sudo tail -f /var/log/odoo/odoo.log`
- Verify RDS connectivity from EC2 instances.
- Check security group rules if you can't access Odoo through the ALB.

## Contributing

Feel free to submit issues or pull requests to improve this deployment guide.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.