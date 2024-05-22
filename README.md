# Automating-EBS-Snapshots-with-AWS-Boto3

![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/049136a3-3e2c-42a6-b862-dde3b0bbe3fa)

# Problem Statement
An organization operating in a cloud-based infrastructure, relying on Amazon Web Services(AWS), needs an efficient solution to take daily snapshots of its Amazon Elastic Block Store(EBS) volumes. Currently, the process of creating snapshots is manual, time-consuming, and error-prone. The organization seeks to implement an automated system for taking daily EBSvolume snapshots to improve data backup and recovery processes and ensure business continuity.

AWS Services used;

1. IAM Role
2. EventBridge
3. SNS
4. Lambda Function

# 1. Create IAM role

Open the Identity and Access Management console and create a new role. The role will give lambda permission to create EBS snapshots and publish SNS notifications. Select AWS service and Use case as lambda, add permissions, and create the role.

![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/d569e5dd-7932-414d-a5bc-eb098c047ef8)

# 2. Create an EventBridge rule

Use this code to create an EventBridge rule called ‘DailySnapshotRule’ scheduled to run every day at 7:00 pm.

```
import boto3

eventbridge_client = boto3.client('events')

response = eventbridge_client.put_rule(
    Name='DailySnapshotRule',
    ScheduleExpression='cron(0 19 * * ? *)',
    State='ENABLED',
)
```



