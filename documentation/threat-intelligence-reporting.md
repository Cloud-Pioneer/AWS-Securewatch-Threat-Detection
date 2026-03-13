# Threat Intelligence Reporting
SecureWatch includes an automated threat intelligence reporting system.

# How Reports Are Generated
- EventBridge triggers a scheduled Lambda function.
- Lambda queries CloudWatch Logs Insights.
- The query identifies the most frequent attacker IP addresses.
- A report is generated.
- SNS sends the report via email.

# Example Output
Top Attacker IPs
185.156.73.182 – 34 attempts
202.61.197.56 – 21 attempts
165.154.172.15 – 19 attempts

These reports help security teams understand attack trends targeting their infrastructure.
