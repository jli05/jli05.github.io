---
title: An AWS Lambda function to start EC2 Instance
layout: post
---

```python
import boto3
region = 'eu-west-1'
instances = ['i-xxxxxxxxxxxxxxxxx']
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
