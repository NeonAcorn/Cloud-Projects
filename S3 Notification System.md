#### **1. Create an S3 Bucket**
![[Pasted image 20250731111400.png]]
#### **2. Create an SNS Topic**
![[Pasted image 20250731111100.png]]
	We are going to keep it a `Standard` topic then give it a name like `notification-topic`
#### **3. Create an SNS Topic**
![[Pasted image 20250731111150.png]]
	After that create a subscription for the topic, I'll name it `notification-subscription`
#### **4. Create an SQS Queue**
![[Pasted image 20250731111014.png]]
	We are going to keep it a `Standard` queue then give it a name like `notification-queue`

#### **5. Create an Lambda function**
![[Pasted image 20250731112353.png]]
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
-
	After making those changes I will deploy the code

![[Pasted image 20250731110912.png]]
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
![[Pasted image 20250731113040.png]]
#### **7. Upload a file to the S3 Bucket**
![[Pasted image 20250731113327.png]]
	If all is well you should receive an email.