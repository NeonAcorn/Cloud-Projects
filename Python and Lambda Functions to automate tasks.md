
**The Problem:**

```
A Client drops many files into an S3 bucket we own. We want to organize the files by day. Write a lambda script to trigger when a file lands in an S3 bucket, and move it to a folder named YYYYMMDD/filename based on when the file was created in the S3 bucket
```

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-236.png?w=1024)

I created an S3 bucket

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-237.png?w=1024)

Files have been added to the bucket for us to sort

```
# organize-s3-objects.py

import boto3
from datetime import datetime

today = datetime.today()

today

todays_date = today.strftime("%Y%m%d")

todays_date

s3_client = boto3.client('s3')

bucket_name = "km-organize-s3-objects"

list_objects_response = s3_client.list_objects_v2(Bucket=bucket_name)

list_objects_response

get_contents = list_objects_response.get("Contents")

get_contents

get_all_s3_objects_and_folder_names = []

for item in get_contents: 
    s3_object_name = item.get("Key")

    get_all_s3_objects_and_folder_names.append(s3_object_name)

get_all_s3_objects_and_folder_names

directory_name = todays_date + "/"

directory_name

if directory_name not in get_all_s3_objects_and_folder_names:
    s3_client.put_object(Bucket=bucket_name, Key=(directory_name))
```

Using Amazons boto3 along with the documentation for it we can see the variables we can use to achieve this

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-238.png?w=1024)

After running the code we can see the folder has been created in the S3 bucket for the current day

```
for item in get_contents:
    object_creation_date = item.get("LastModified").strftime("%Y%m%d") + "/"
    object_name = item.get("Key")

    if object_creation_date == directory_name and "/" not in object_name:
        s3_client.copy_object(Bucket=bucket_name, CopySource=bucket_name+"/"+object_name, Key=directory_name+object_name)
```

We will use this code to copy all the files that have been modified today into the folder for today

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/08/image-239.png?w=1024)

```
s3_client.delete_object(Bucket=bucket_name, Key=object_name)
```

Adding this line to the last argument will allow for us to delete all the files that are not in the folder

```
# organize-s3-objects.py

import boto3
from datetime import datetime

today = datetime.today()
todays_date = today.strftime("%Y%m%d")

s3_client = boto3.client('s3')

bucket_name = "km-organize-s3-objects"
list_objects_response = s3_client.list_objects_v2(Bucket=bucket_name)

get_contents = list_objects_response.get("Contents")

get_all_s3_objects_and_folder_names = []

for item in get_contents: 
    s3_object_name = item.get("Key")

    get_all_s3_objects_and_folder_names.append(s3_object_name)

directory_name = todays_date + "/"

if directory_name not in get_all_s3_objects_and_folder_names:
    s3_client.put_object(Bucket=bucket_name, Key=(directory_name))

for item in get_contents:
    object_creation_date = item.get("LastModified").strftime("%Y%m%d") + "/"
    object_name = item.get("Key")

    if object_creation_date == directory_name and "/" not in object_name:
        s3_client.copy_object(Bucket=bucket_name, CopySource=bucket_name+"/"+object_name, Key=directory_name+object_name)
        s3_client.delete_object(Bucket=bucket_name, Key=object_name)
```

I have cleaned up the code and converted it to a python script after having written it first in Jupyter Notebook format

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image.png?w=866)

Next I went ahead and created another S3 bucket that will be used to upload out python code to that was placed in a .zip file

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-1.png?w=1024)

In IAM I created a new role for lambda called organize-s3-objects that will have s3fullaccess and AWSlambdabasicexecutionrole for the permissions

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-2.png?w=1014)

For the Lambda function the name will be **organize-s3-objects** the runtime will be set to **Python** and the architecture is **x86_64** and the role will be the one I just created.

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-3.png?w=806)

For the trigger it will be the S3 bucket we created to store the files. The event will be whenever objects are **PUT** into the bucket and we will acknowledge the recursive invocation

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-4.png?w=1024)

I have uploaded the python code from the S3 bucket into the lambda function

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-5.png?w=1010)

We want to make sure that the handler is updated to the name of the script. Just remove the first part **lambda_function** and replace it the with script name

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-8.png?w=1024)

We can see the handler has been updated

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-9.png?w=1024)

Now I created a test event called **organize-s3-objects-event** and tested it. At first it was giving a timeout error was was resolved by just increasing the time out window

![](https://kylemichaelit.wordpress.com/wp-content/uploads/2024/09/image-10.png?w=1024)

When we add files to our S3 bucket we can see they now get organized into folders for their date uploaded