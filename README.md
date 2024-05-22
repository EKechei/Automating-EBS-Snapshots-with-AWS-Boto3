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

Below is my created EventBridge rule.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/12c8d878-f7bc-4a17-97b6-150b04b53bc5)


# 3. Create SNS Topic

In the Amazon SNS console, choose Create Topic, enter your topic name, and then create the topic.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/8475cc36-1cc0-4d96-b962-605d17403c00)


Once the topic is created, create a subscription, choose email as the protocol, and enter the email address you would like to receive notifications from. On your email address inbox confirm subscription.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/921a897b-3f33-4cb5-96ab-6de16688e7af)


Copy the SNS Topic ARN, you will paste it on your lambda function.

# 4. Create a Lambda Function

Open the AWS Lambda console and click the Create function tab, on the Permissions tab, select the role you created earlier.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/34de8e5e-0001-4245-b602-6edf9a2fd1b0)


Paste your code that is going to automate the creation of EBS snapshots and also publish SNS notifications. Do not forget the SNS Topic ARN you copied earlier.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/dd303d68-ad37-41ed-8c2c-6a49b9aec4e7)



Add trigger and select EventBridge, In the Trigger configuration, select Existing rules, choose the EventBridge rule you created earlier, and then add the trigger.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/9d456c7c-6a77-48cb-a436-2c2d02309298)



Test the lambda function.

My automated snapshot was created successfully and I also received an email notification.


![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/850788c3-a09e-4afb-9750-6b65ca75e112)
![image](https://github.com/EKechei/Automating-EBS-Snapshots-with-AWS-Boto3/assets/128794751/1ea2dd3a-bdc0-4a9e-bfd2-de2ffd6b1852)








