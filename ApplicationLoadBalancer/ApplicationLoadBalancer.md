# Creating an Application Load Balancer on AWS
This tutorial will guide you through the process of creating an Application Load Balancer on AWS and setting up an Auto Scaling group to ensure high availability and scalability.

## Prerequisites
Before you begin, make sure you have the following:

- An AWS account with appropriate permissions to create a load balancer, EC2 instances, security groups, and Auto Scaling groups.
- An Amazon Machine Image (AMI) for the EC2 instances. We will use the Amazon Linux 2 AMI (free tier eligible) with a t2.micro instance type.
- A target group to register the EC2 instances with.

## Steps
### 1. Create a security group for the load balancer
### 2. Add inbound rules to the security group
Add HTTP - All Traffic.

### 3. Create the Application Load Balancer
Use the AWS Management Console to create the Application Load Balancer. Here are the values we'll use:

- Name: my-application-load-balancer
- Scheme: internet-facing
- IP address type: ipv4
- Listeners:
  - Protocol: HTTP
  - Port: 80
  - Default action: Forward to target group
- Availability zones: choose at least two
- Security group: my-load-balancer-security-group

### 4. Create the EC2 instances
Launch EC2 instances with the Amazon Linux 2 AMI and a t2.micro instance type.

### 5. Create a target group for the instances
Use the AWS Management Console to create a target group for the instances. Here are the values we'll use:

- Name: my-target-group
- Protocol: HTTP
- Port: 80
- VPC: choose your VPC
- Health checks:
  - Protocol: HTTP
  - Path: /health-check
  - Port: traffic-port
  - Healthy threshold: 3
  - Unhealthy threshold: 3
  - Timeout: 5 seconds
  - Interval: 30 seconds

### 6. Register the EC2 instances with the target group
Use the AWS Management Console to register the EC2 instances with the target group.

### 7. Create an Auto Scaling group
Set up an Auto Scaling group to automatically adjust the number of EC2 instances based on the demand. Here are the values we'll use:

- Name: my-auto-scaling-group
- Launch configuration: Select the same AMI and instance type used for the EC2 instances
- Min size: 1
- Max size: 10
- Desired capacity: 2
- Target group: my-target-group
- Health check type: ELB
- Health check grace period: 300 seconds
- Subnets: Select the same subnets used for the ALB

### 8. Verify that the instances are responding to health checks
SSH into an instance and run a command to test the health check endpoint:
