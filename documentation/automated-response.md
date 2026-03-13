# Automated Incident Response
SecureWatch uses AWS Lambda to automatically respond to suspicious network activity.

# Lambda Response Workflow
When a CloudWatch alarm triggers:

-  SNS sends a message to Lambda
- Lambda parses the log entry
- The attacker source IP is extracted
- The EC2 security group is updated
- A rule is added to block the attacker

# Benefits
This approach prevents attackers from repeatedly targeting the same infrastructure.
The automated response reduces manual intervention and speeds up incident mitigation.
