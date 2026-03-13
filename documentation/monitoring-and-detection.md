# Monitoring and Threat Detection
SecureWatch uses AWS logging and monitoring services to identify suspicious activity.

# Network Monitoring
VPC Flow Logs capture metadata about network traffic including:
- source IP
- destination IP
- source port
- destination port
- action (ACCEPT or REJECT)

# Detection Strategy
Rejected traffic can indicate:
- port scanning
- unauthorized access attempts
- brute force login attempts
CloudWatch Metric Filters analyze logs and count the number of rejected connections.

# Alerting
CloudWatch Alarms trigger when rejected traffic exceeds a defined threshold.
This ensures administrators are notified immediately when suspicious patterns appear.
