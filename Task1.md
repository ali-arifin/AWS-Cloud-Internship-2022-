# Task No. 1
## :memo: Launched EC2 instances, domain-joined EC2 to Active Directory & created users
:heavy_check_mark: **Launched EC2s by AWS Management Console:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/f242ab25-7476-437b-8666-3e74ac69ab2c)
#  
:heavy_check_mark: **Launched EC2s by CloudFormation code:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/95c8014e-3669-4f7c-b196-0b37461c9904)
#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0f34c5ae932e6f0e4
      Tags:
        - Key: Name
          Value: Ali's instance
```
#
:heavy_check_mark: **Output of CloudFormation:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/19c4e1b0-1672-4c72-a5ea-4d5fb4b31e43)
#
:heavy_check_mark: **Manually domain-joined Linux EC2 to Active Directory:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/7027b72e-e5c2-401e-ae31-9cb3191132c9)
#
:heavy_check_mark: **Commands used:**
```linux
sudo realm join -U admin@ali.com ali.com
realm list
```
#
:heavy_check_mark: **Domain-joined EC2 by CloudFormation code:**
```yaml
Parameters:
  AMI:
    Type: String
    Description: AMI of EC2
  KeyPair:
    Type: AWS::EC2::KeyPair::KeyName
    Description: KeyPair for EC2 
  PublicSubnet:
    Type: AWS::EC2::Subnet::Id
    Description: Subnet for EC2
  VPC:	
    Type: AWS::EC2::VPC::Id
    Description: VPC for EC2
  InstanceType:
    Type: String
    Default: t2.micro
  ADDirectoryId:
    Type: String
    Description: Active DirectoryId
  ADDirectoryName:
    Type: String
    Description: Active Directory FQDN
  ADDnsIpAddresses1:
    Type: String
    Description: Active Directory DNS 1
  ADDnsIpAddresses2:
    Type: String
    Description: Active Directory DNS 2
Resources:
  mySSMdocument:
    Type: AWS::SSM::Document
    Properties:
      Content:
        schemaVersion: '1.2'
        description: Join instances to Directory Service domain.
        parameters:
          directoryId:
            type: String
            description: "(Required) The ID of the AWS Directory Service directory."
          directoryName:
            type: String
            description: "(Required) The name of the directory."
          dnsIpAddresses:
            type: StringList
            default: []
            description: "(Optional) The IP addresses of the DNS servers in the directory."
        runtimeConfig:
          aws:domainJoin:
            properties:
              directoryId: "{{ directoryId }}"
              directoryName: "{{ directoryName }}"
              dnsIpAddresses: "{{ dnsIpAddresses }}"
  myEC2InstanceSSM:
    Type: AWS::EC2::Instance
    Properties:
      IamInstanceProfile:
        Ref: myInstanceProfile
      SsmAssociations:
      - DocumentName:
          Ref: mySSMdocument
        AssociationParameters:
        - Key: directoryId
          Value:
          - Ref: ADDirectoryId
        - Key: directoryName
          Value:
          - Ref: ADDirectoryName
        - Key: dnsIpAddresses
          Value:
          - Ref: ADDnsIpAddresses1
          - Ref: ADDnsIpAddresses2
      KeyName:
        Ref: KeyPair
      ImageId:
        Ref: AMI
      InstanceType:
        Ref: InstanceType
      Tags:
      - Key: Name
        Value: myEC2InstanceSSM
      SubnetId:
        Ref: PublicSubnet
      SecurityGroupIds:
      - Fn::GetAtt:
        - InstanceSecurityGroup
        - GroupId
  myInstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Roles:
      - DemoEC2SSMRole
      InstanceProfileName: myEC2SSMRole
  myEC2SSMRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
        - Effect: Allow
          Principal:
            Service:
            - ec2.amazonaws.com
          Action:
          - sts:AssumeRole
      ManagedPolicyArns:
      - arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM
      RoleName: DemoEC2SSMRole
  InstanceSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow rdp access
      VpcId:
        Ref: VPC
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: '3389'
        ToPort: '3389'
        CidrIp: 0.0.0.0/0

```
#
:heavy_check_mark: **Created two users for a Linux EC2:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/8f6a9680-bd27-43e5-a29e-eb25302405b3)
#
:heavy_check_mark: **Created two users for a Windows EC2:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/3fcdabb1-75e9-4ddf-821e-6153d8a501ae)
