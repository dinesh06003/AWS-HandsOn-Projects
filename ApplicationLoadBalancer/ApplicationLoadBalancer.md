# Creating an Application Load Balancer and Auto Scaling Group on AWS
This tutorial will guide you through the process of creating an Application Load Balancer on AWS and setting up an Auto Scaling group to ensure high availability and scalability.

## Prerequisites
Before you begin, make sure you have the following:

- An AWS account with appropriate permissions to create a load balancer, EC2 instances, security groups, and Auto Scaling groups.
- An Amazon Machine Image (AMI) for the EC2 instances. We will use the Amazon Linux 2 AMI (free tier eligible) with a t2.micro instance type.
- A target group to register the EC2 instances with.

## Steps
### 1. Create a security group for the load balancer
Security group Name: my-load-balancer-security-group
Add inbound rules to the security group
Add HTTP - All Traffic.

### 2. Create the EC2 instances
Launch EC2 instances with below configuration:
- AMI: Amazon Linux 2 AMI/ 2023 AMI.
- Instance Type: t2.micro.
- Key Pair: Proceed without a key pair.
- Allow HTTP and SSH traffic.
- In Advanced details -> User data
copy and paste below code in the user data
```
#!/bin/bash
# Use this for your user data (script from top to bottom)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

### 3. Create a target group for the instances
Use the AWS Management Console to create a target group for the instance and register the EC2 instances with the target group. Here are the values we'll use:

- Name: my-target-group-for-ALB
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
click on next
Select EC2 instance that you create above.
click on include as pending below.
click on create target group.


### 4. Create the Application Load Balancer
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

### 5. Create an Launch Template
Click the “Create launch template” button. This will open the "Create launch template" page where you can specify the details for your template.
Template Details:
- Launch Template Name: ALB-Template-AutoScaling.
- AMI: Amazon Linux 2 AMI/ 2023 AMI.
- Instance Type: t2.micro.
- Key Pair: Proceed without a key pair.
- **Network settings**:
  - Security Groups: Select security group which is already created for your instance or create new one that allows HHTP and SSH Traffic.
- **Advanced Details**:
  - **User Data** (optional): copy paste the same User data provided in the ec2 instance creation section.
  - **IAM role**: Attach an IAM role if necessary.

### 6. Create an Auto Scaling group
Set up an Auto Scaling group to automatically adjust the number of EC2 instances based on the demand. Here are the values we'll use:

- Name: my-auto-scaling-group
- Launch Template: Select the launch Template ALB-Template-AutoScaling
- Min size: 1
- Max size: 5
- Desired capacity: 2
- Target group: my-target-group-for-ALB
- Health check type: ELB
- Health check grace period: 100 seconds
- Subnets: Select the same subnets used for the ALB

### 7. Verify that the instances are responding to health checks
copy the DNS name in the my-application-load-balancer and paste in the browset to check ALB is working fine or not.

### Conclusion
Congratulations! You have successfully created an Application Load Balancer on AWS.


### Don't forget to close/delete resources that you created while doing this Handson to avoid any additional cost.