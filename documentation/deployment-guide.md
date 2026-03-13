# SecureWatch Deployment Guide
This guide explains how to deploy the SecureWatch automated threat detection system in AWS.

# Step 1 – Launch EC2 Instance
Launch an Amazon EC2 instance that will act as the monitored web server.

Security group rules:
Allow
- Port 22 (SSH)
- Port 80 (HTTP)
All other traffic should be denied.

# Step 2 – Enable VPC Flow Logs

-  Navigate to VPC console
-  Select your VPC
-  Enable Flow Logs
-  Send logs to CloudWatch Logs
This allows monitoring of network traffic.

# Step 3 – Configure CloudWatch Metric Filter
Create a metric filter to detect rejected connections.
Filter pattern:
REJECT
This will count suspicious connection attempts.

# Step 4 – Create CloudWatch Alarm
Trigger the alarm when the metric exceeds a threshold.
Example threshold:
5 rejected connections within 5 minutes.

# Step 5 – Configure SNS Notifications
Create an SNS topic and subscribe your email.
The alarm will send alerts when suspicious traffic is detected.

# Step 6 – Deploy Lambda Response Function
Deploy the Lambda function responsible for blocking attacker IPs.
Required permissions:
- ec2:AuthorizeSecurityGroupIngress
- ec2:DescribeSecurityGroups

# Step 7 – Configure EventBridge Scheduler
Create an EventBridge schedule to trigger the threat intelligence report Lambda every 24 hours.

This automates security reporting.
