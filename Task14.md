# Task No. 14
## :memo: Created a Lambda function to launch EC2
:heavy_check_mark: **Created a Python Lambda function to launch EC2:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/caf297d6-8d01-4507-a6d4-1dbdd544e6d8)


#
:heavy_check_mark: **Lambda code:**
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
:heavy_check_mark: **Execution result of Lambda:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/59dd0dc2-cceb-4797-8ad2-002e98d4cd51)


#
:heavy_check_mark: **EC2 launched by Lamda:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/4b0759cf-bde6-4852-9bb7-c68e9ae6b7e0)

