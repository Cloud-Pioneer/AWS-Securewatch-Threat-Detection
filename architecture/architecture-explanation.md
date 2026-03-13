# SecureWatch System Architecture
The SecureWatch architecture is designed to automatically detect suspicious network activity and respond without human intervention.
The system uses AWS managed services to build a fully serverless security monitoring pipeline.

# Architecture Flow

-  Internet traffic reaches the EC2 web server.
-  VPC Flow Logs capture metadata about the traffic.
-  Flow logs are sent to CloudWatch Logs.
-  CloudWatch Metric Filters detect rejected traffic patterns.
-  CloudWatch Alarm triggers when suspicious traffic exceeds threshold.
-  SNS sends security alert notifications.
-  Lambda parses the log entry and extracts the attacker IP.
-  Lambda updates the EC2 security group to block the attacker.
-  EventBridge triggers daily threat intelligence reports.

# Security Benefits
This architecture provides:

- automated detection
- real-time alerting
- automatic incident response
- continuous threat intelligence reporting
