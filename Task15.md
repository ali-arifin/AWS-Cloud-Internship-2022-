# Task No. 15 (a)
## :memo: Scheduled a Lambda function (that creates an EC2) using EventBridge Rule
:heavy_check_mark: **EventBridge Rule that targets a Lambda function:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/33fe4a47-b250-46b8-996c-a905e48a687a)



#
:heavy_check_mark: **Lambda code that creates an EC2:**
```python
import os
import boto3

AMI = os.environ['AMI']
INSTANCE_TYPE = os.environ['INSTANCE_TYPE']
KEY_NAME = os.environ['KEY_NAME']
SUBNET_ID = os.environ['SUBNET_ID']

ec2 = boto3.resource('ec2')


def lambda_handler(event, context):

    instance = ec2.create_instances(
        ImageId=AMI,
        InstanceType=INSTANCE_TYPE,
        KeyName=KEY_NAME,
        SubnetId=SUBNET_ID,
        MaxCount=1,
        MinCount=1
    )

    print("New instance created:", instance[0].id)
```

#
:heavy_check_mark: **Dashboard showing 1 invocation of Lambda:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/286aca21-58cc-4e82-b80f-90aa280795b0)
#
# Task No. 15 (b)
## :memo: Scheduled a Lambda function (that creates an EC2) and got the notification via SNS using Step Functions
:heavy_check_mark: **Created a serverless workflow that schedules Lambda at a specific time and sends notification via SNS:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/5e17d3f4-2492-4cf9-95ec-25f433e68099)

