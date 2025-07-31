#### **1. Create an S3 Bucket**
<img width="428" height="164" alt="Pasted image 20250731111400" src="https://github.com/user-attachments/assets/5dbb3c4f-95cc-4c1a-a461-7499f1017c09" />

#### **2. Create an SNS Topic**
<img width="1213" height="541" alt="Pasted image 20250731111100" src="https://github.com/user-attachments/assets/0f1dc676-96da-4720-b7f3-6e756451a6f6" />
	We are going to keep it a `Standard` topic then give it a name like `notification-topic`

#### **3. Create an SNS Subscription**
<img width="1188" height="483" alt="Pasted image 20250731111150" src="https://github.com/user-attachments/assets/78426e62-8fdc-4742-bafd-20c0cb36e05c" />
	After that create a subscription for the topic, I'll name it `notification-subscription`

#### **4. Create an SQS Queue**
<img width="1320" height="485" alt="Pasted image 20250731111014" src="https://github.com/user-attachments/assets/fdbc2543-867b-4d0e-a741-50ebdcef57c6" />
	We are going to keep it a `Standard` queue then give it a name like `notification-queue`

#### **5. Create an Lambda function**
<img width="1140" height="800" alt="Pasted image 20250731112353" src="https://github.com/user-attachments/assets/35b4a84a-7f8d-4d78-ab04-b6332ee2a591" />
When making the function I will be using `Python` and making sure it will be creating a new role that I will edit after


This is the code we will be using for the function. I will be updating `sns_topic_arn` and `sqs_queue_url` with the _ARN_ and _URL_ of my **SNS topic** and **SNS queue URL**

```python
import json
import boto3

s3_client = boto3.client('s3')
sns_client = boto3.client('sns')
sqs_client = boto3.client('sqs')


def lambda_handler(event, context):
    sns_topic_arn = 'arn:aws:sns:us-east-1:452529558025:s3-topic'

    # Define the SQS queue URL
    sqs_queue_url = 'https://sqs.us-east-1.amazonaws.com/452529558025/notification-queue'

    # Process S3 event records
    for record in event['Records']:
        print(event)
        # Extract S3 bucket and object information
        s3_bucket = record['s3']['bucket']['name']
        s3_key = record['s3']['object']['key']
        
        # Example: Sending metadata to SQS
        metadata = {
            'bucket': s3_bucket,
            'key': s3_key,
            'timestamp': record['eventTime']
        }
        
        # Send metadata to SQS
        sqs_response = sqs_client.send_message(
            QueueUrl=sqs_queue_url,
            MessageBody=json.dumps(metadata)
        )
        
        # Example: Sending a notification to SNS
        notification_message = f"New file uploaded to S3 bucket '{s3_bucket}' with key '{s3_key}'"
        
        sns_response = sns_client.publish(
            TopicArn=sns_topic_arn,
            Message=notification_message,
            Subject="File Upload Notification"
        )

    return {
        'statusCode': 200,
        'body': json.dumps('Processing complete')
    }
```

After making those changes I will deploy the code

<img width="1401" height="649" alt="Pasted image 20250731110912" src="https://github.com/user-attachments/assets/80e55a9c-50d1-4653-942a-1f561a856d62" />
For the permissions you can make it with least privilege for security reason however for this I am just going to add `AmazonSNSFullAccess`, `AmazonSQSFullAccess` and I'll add a permission to access my bucket and the contents inside of it.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3BucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::notification-system-km",
                "arn:aws:s3:::notification-system-km/*"
            ]
        }
    ]
}
```

#### **6. Add a Trigger in the Lambda Console**

1. Go to the **AWS Lambda console**.
2. Select your Lambda function.
3. Under the **“Configuration”** tab, click **“Triggers”** in the sidebar.
4. Click **“+ Add trigger”**.
5. Choose **“S3”** from the trigger options.
6. Configure:
    - **Bucket**: Select your target S3 bucket.
    - **Event type**: Choose `Object Created (All)` to trigger on upload.
    - **Prefix/Suffix** (optional): Use to limit triggers (e.g. only `.jpg` files).
7. Click **“Add”**.

Go over to the S3 bucket and click on `properties` to make sure you see that there is an Event notification for the bucket leading back to the lambda function.
<img width="1632" height="294" alt="Pasted image 20250731113040" src="https://github.com/user-attachments/assets/953c4731-7316-4b2d-a0b4-19759d06c034" />

#### **7. Upload a file to the S3 Bucket**
<img width="557" height="129" alt="Pasted image 20250731113327" src="https://github.com/user-attachments/assets/461cb213-5491-4735-838f-0459ccc4537b" />

If all is well you should receive an email.
