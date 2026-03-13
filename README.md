# AWS-Securewatch-Threat-Detection
Automated AWS cloud security monitoring system that detects suspicious network traffic, blocks attacker IPs, and generates daily threat intelligence reports using VPC Flow Logs, CloudWatch, Lambda, SNS, and EventBridge.
# AWS SecureWatch – Automated Cloud Threat Detection & Response System

# Project Overview

Modern businesses host critical applications and customer data in the cloud. While cloud platforms provide powerful infrastructure, publicly exposed servers are constantly scanned and probed by automated attackers across the internet.

Organizations must detect suspicious traffic, respond quickly to potential threats, and maintain visibility into security events occurring in their cloud environments.

However, many small and medium-sized companies lack automated monitoring systems and rely on manual log inspection, which is inefficient and reactive.

The project solves this problem by implementing an automated threat detection and response system that continuously monitors network traffic, detects suspicious activity, automatically blocks attacker IP addresses, and generates daily threat intelligence reports.

This project demonstrates how cloud-native security automation can be implemented using serverless AWS services.

# Business Problem

Public-facing cloud servers are constantly targeted by automated bots, scanners, and malicious actors attempting to exploit vulnerabilities.

Common threats include:

* Port scanning
* Brute force login attempts
* Unauthorized connection attempts
* Reconnaissance attacks

Without automated monitoring:

* Security teams must manually review logs
* Suspicious activity may go unnoticed
* Response times are slow
* Attackers can repeatedly target the same infrastructure

Organizations require an automated system that can:

1. Continuously monitor network traffic
2. Detect suspicious connection attempts
3. Alert administrators in real time
4. Automatically respond to malicious activity
5. Produce security intelligence reports

# Solution

AWS SecureWatch implements a fully automated cloud security monitoring pipeline.

The system continuously analyzes network traffic captured through VPC Flow Logs and uses log analytics to identify suspicious connection attempts.

When suspicious traffic is detected:

1. A monitoring rule identifies the event
2. A security alert is triggered
3. A serverless function extracts the attacker IP
4. The attacker IP is automatically blocked at the network level
5. Daily threat intelligence reports are generated

The solution leverages event-driven architecture and serverless automation to eliminate manual security monitoring.

# Architecture Overview

The system is built entirely using managed AWS services.

Architecture workflow:

Internet Traffic
        ↓
VPC Flow Logs capture network traffic
        ↓
CloudWatch Logs stores and analyzes traffic
        ↓
Metric Filters detect suspicious patterns
        ↓
CloudWatch Alarm triggers alert
        ↓
SNS sends notification
        ↓
Lambda parses attacker IP
        ↓
Security Group updated automatically
        ↓
Attacker IP blocked

Additionally, a scheduled automation pipeline generates daily threat intelligence reports.

EventBridge Scheduler
        ↓
Lambda executes CloudWatch Logs Insights query
        ↓
Top attacker IPs identified
        ↓
SNS sends threat intelligence report

# AWS Services Used

The project demonstrates integration between multiple AWS services:

| Service                   | Purpose                                        |
| ------------------------- | ---------------------------------------------- |
| Amazon EC2                | Simulated production server being monitored    |
| VPC Flow Logs             | Capture network traffic metadata               |
| Amazon CloudWatch Logs    | Store and analyze network logs                 |
| CloudWatch Metric Filters | Detect suspicious traffic patterns             |
| CloudWatch Alarms         | Trigger security alerts                        |
| Amazon SNS                | Send alert notifications                       |
| AWS Lambda                | Automate threat response and reporting         |
| Amazon EventBridge        | Schedule automated threat intelligence reports |

# Key Features

# Automated Threat Detection
The system analyzes network traffic logs to identify rejected connection attempts that may indicate malicious activity such as port scanning or unauthorized access attempts.

# Real-Time Security Alerts
When suspicious traffic exceeds a defined threshold, an alert is automatically triggered and delivered to administrators via email notifications.

# Automated Incident Response
A Lambda function extracts the attacker IP address from the log event and automatically updates the server’s security group to block the source IP.
This prevents the attacker from repeatedly targeting the server.

# Threat Intelligence Reporting
A scheduled automation pipeline runs daily log analysis queries and produces a summary report identifying the most frequent attacker IP addresses.
These reports provide visibility into ongoing attack patterns targeting the infrastructure.

# Example Threat Intelligence Output
Example daily report generated by the system:

SecureWatch Daily Threat Report
Top Attacker IPs
185.156.73.182 – 34 connection attempts
202.61.197.56 – 21 connection attempts
165.154.172.15 – 19 connection attempts

This information helps administrators understand the nature and frequency of attacks targeting their infrastructure.

# Conclusion

AWS SecureWatch demonstrates how cloud-native tools can be used to build an automated security monitoring and response system without managing dedicated infrastructure.
By leveraging managed AWS services and serverless automation, organizations can improve their security posture while minimizing operational overhead.

This project illustrates practical cloud security engineering techniques and real-world automation patterns used in modern cloud environments.
