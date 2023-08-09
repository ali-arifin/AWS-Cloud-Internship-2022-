# Task No. 4
## :memo: Created S3 bucket, uploaded files via FileZilla, created two IAM users - one having read-only & the other having read-and-write permission
:heavy_check_mark: **Uploaded files in S3 bucket via FileZilla:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/40a4869c-cf38-42e8-8f67-ad2ae690f6bd)



#  
:heavy_check_mark: **Two IAM users created, User1 has read-only access, User2 has read-and-write access:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/23b61917-6116-4fcd-a6b9-f0a554479a79)



#
:heavy_check_mark: **User1 unable to rename a file:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/b6c556f3-61ab-45f8-98ab-d4d0fd0f3a89)



#
:heavy_check_mark: **User1 unable to upload file:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/0d286aa6-c0d0-4410-b738-f1dde369a15e)



#
:heavy_check_mark: **User2 can rename files (has full access):**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/89f187be-e830-487b-9a69-dc1a5c6201bb)




#
:heavy_check_mark: **Repeating the task by CloudFormation:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/ed26d7d0-9e58-489e-87a3-479d7e586ffd)



#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:
  S3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      AccessControl: Private
      BucketName: ali-arifin-bucket-01
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
      VersioningConfiguration:
        Status: Suspended 

  User3:
    Type: 'AWS::IAM::User'
    Properties:
      UserName: user3
      LoginProfile:
        Password: abcd@1234
        PasswordResetRequired: no
      ManagedPolicyArns: 
        - arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

  User4:
    Type: 'AWS::IAM::User'
    Properties:
      UserName: user4
      LoginProfile:
        Password: abcd@5678
        PasswordResetRequired: no
      ManagedPolicyArns: 
        - arn:aws:iam::aws:policy/AmazonS3FullAccess
```
