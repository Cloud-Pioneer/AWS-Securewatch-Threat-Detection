# Security Analysis
SecureWatch demonstrates a proactive cloud security monitoring strategy.

# Attack Types Observed
During testing, several types of suspicious network activity were observed including:
- port scanning attempts
- unauthorized HTTPS connection attempts
- repeated access from known scanning IP ranges

# Defensive Measures Implemented
The system protects infrastructure using multiple layers:
- Security Groups restrict access
- VPC Flow Logs provide visibility
- CloudWatch detects anomalies
- Lambda automates incident response

# Scalability Considerations
For large organizations, the architecture could be extended to:
- centralize logs across multiple AWS accounts
- integrate with AWS WAF
- store threat intelligence in Amazon S3
- visualize attacks using security dashboards
