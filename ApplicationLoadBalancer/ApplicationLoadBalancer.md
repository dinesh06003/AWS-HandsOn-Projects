# Creating an Application Load Balancer on AWS
This tutorial will guide you through the process of creating an Application Load Balancer on AWS.

## Prerequisites
Before you begin, make sure you have the following:

- An AWS account with appropriate permissions to create a load balancer, EC2 instances, and security groups.
- An Amazon Machine Image (AMI) for the EC2 instances. In this tutorial, we will use the Amazon Linux 2 AMI (free tier eligible) with a t2.micro instance type.
- A target group to register the EC2 instances with.
## Steps
### 1. Create a security group for the load balancer

### 2. Add inbound rules to the security group
Add HTTP - All Traffic

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

### 7. Verify that the instances are responding to health checks
SSH into an instance and run a command to test the health check endpoint:
```
curl http://localhost:80/health-check
```
### 8. Update the security group for the instances
Verify that the security group attached to the instances allows inbound traffic from the load balancer on the health check port (typically port 80 or 443).

### 9. Update the network ACL rules
Verify that the network ACL attached to the subnet where the instances are located allows inbound traffic from the load balancer on the health check port.

### 10. Update the health check URL
Verify that the health check URL configured on your load balancer is correct and matches the URL of your application. If your application is running on a non-standard port, make sure to include the port number in the health check URL.

### Conclusion
Congratulations! You have successfully created an Application Load Balancer on AWS.