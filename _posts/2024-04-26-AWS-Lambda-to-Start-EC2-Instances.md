---
title: AWS Lambda function to start EC2 instances
layout: post
---

Example Lambda function:

```python
import boto3
region = '...'
instances = ['...']
ec2 = boto3.client('ec2', region_name=region)

import json

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    return {
        'statusCode': 200,
        'body': json.dumps('started your instances: '
                           + str(instances))
    }
```

This Lambda function need be associated with an IAM Role, with the policy below among its policy set:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances"
            ],
            "Resource": [
                "arn:aws:ec2:{region}:{account_id}:instance/{instance}"
            ]
        }
    ]
}
```

To check for Lambda function running logs, go to CloudWatch/Logs for output and error messages.

If this Lambda function need be regularly evoked, create a EventBridge Rule in AWS. The expression is similar to cron expression for setting the times and commands.
