---
title: An AWS Lambda function to subscribe to SNS
layout: post
---

Decode depending on the "content-type".

```python
import boto3

from base64 import b64decode
from urllib.parse import parse_qs
import json


def lambda_handler(event, context):
    sns = boto3.client('sns')

    if event.get('isBase64Encoded'):
        event_body = b64decode(event['body']).decode()
    else:
        event_body = event['body']

    if event['headers']['content-type'] == 'application/x-www-form-urlencoded':
        qs = parse_qs(event_body)
        [email] = qs['email']
    elif event['headers']['content-type'] == 'application/json':
        event_body = json.loads(event_body)
        email = event_body['email']

    sns.subscribe(
        TopicArn='<topic>',
        Protocol='email',
        Endpoint=email
    )

    return {
        'statusCode': 200,
        'body': json.dumps('The email address has been added.')
    }
```
