# Lambda Automation Functions
SecureWatch uses two AWS Lambda functions.

# Block Attacker IP
This function parses SNS alerts triggered by CloudWatch alarms.
It extracts the attacker IP address from the log event and updates the EC2 security group to block the source IP.
import boto3
import re

ec2 = boto3.client('ec2')

SECURITY_GROUP_ID = "YOUR SECURITY GROUP ID"
MAX_RULES = 20


def lambda_handler(event, context) :

    message = event['Records'][0]['Sns']['Message']

    ip_match = re.search(r'\d+\.\d+\.\d+\.\d+', message)

    if not ip_match:
        return {"status": "No IP found"}

    attacker_ip = ip_match.group(0)
    attacker_cidr = f"{attacker_ip}/32"

    sg = ec2.describe_security_groups(GroupIds=[SECURITY_GROUP_ID])
    rules = sg['SecurityGroups'][0]['IpPermissions']

    blocked_ips = []

    for rule in rules:
        for ip in rule.get('IpRanges', []):
            if 'Blocked by SecureWatch' in ip.get('Description', ''):
                blocked_ips.append(ip['CidrIp'])

    # Check if IP already blocked
    if attacker_cidr in blocked_ips:
        return {
            "status": "IP already blocked",
            "ip": attacker_ip
        }

    # Remove oldest rule if limit reached
    if len(blocked_ips) >= MAX_RULES:

        oldest_ip = blocked_ips[0]

        ec2.revoke_security_group_ingress(
            GroupId=SECURITY_GROUP_ID,
            IpPermissions=[
                {
                    'IpProtocol': '-1',
                    'IpRanges': [{'CidrIp': oldest_ip}]
                }
            ]
        )

    # Add new blocked IP
    ec2.authorize_security_group_ingress(
        GroupId=SECURITY_GROUP_ID,
        IpPermissions=[
            {
                'IpProtocol': '-1',
                'IpRanges': [
                    {
                        'CidrIp': attacker_cidr,
                        'Description': 'Blocked by SecureWatch'
                    }
                ]
            }
        ]
    )

    return {
        "status": "Blocked attacker",
        "ip": attacker_ip
    }

# Threat Intelligence Report
This function runs a CloudWatch Logs Insights query to identify the most frequent attacker IPs.

import boto3
import time

logs = boto3.client('logs')
sns = boto3.client('sns')

LOG_GROUP = "SecureWatch-VPC-Logs"
SNS_TOPIC = "YOUR SNS ARN"

query = """
fields srcAddr, dstPort, action
| filter action="REJECT"
| stats count() as attempts by srcAddr
| sort attempts desc
| limit 5
"""

def lambda_handler(event, context):

    start_query = logs.start_query(
        logGroupName=LOG_GROUP,
        startTime=int(time.time()) - 86400,
        endTime=int(time.time()),
        queryString=query
    )

    query_id = start_query['queryId']

    time.sleep(5)

    response = logs.get_query_results(queryId=query_id)

    results = response['results']

    report = "SecureWatch Daily Threat Report\n\n"

    for r in results:
        ip = r[0]['value']
        attempts = r[1]['value']
        report += f"IP: {ip} | Attempts: {attempts}\n"

    sns.publish(
        TopicArn=SNS_TOPIC,
        Message=report,
        Subject="SecureWatch Security Report"
    )

    return {"status": "Report Sent"}
    
The results are formatted and sent via SNS email as a daily report.
