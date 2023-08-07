# Task No. 2
## :memo: Created Application Load Balancer for multiple web servers in default VPC
:heavy_check_mark: **Two webservers in target group, one stopped intentionally so that Load Balancer routes the request to the second server:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/6c221cd0-b22e-46f0-823e-c9da16b61372)

#  
:heavy_check_mark: **Load balancer routes the request to second server (healthy):**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/ab97cfa1-bf28-4836-990b-9de4132bcf56)

#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:
  MyInstance1:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-05fa00d4c63e32376 
      KeyName: ali-key-pair
      AvailabilityZone: us-east-1d
      SecurityGroupIds:
        - !Ref MySecurityGroup
      Tags:
        - Key: Name
          Value: WebServer1_viaCode
      UserData:
        Fn::Base64: 
          !Sub |
            #!/bin/bash
            yum update -y
            yum install -y httpd.x86_64
            systemctl start httpd.service
            systemctl enable httpd.service
            echo ?Hello World from $(hostname -f)? > /var/www/html/index.html

  MyInstance2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-05fa00d4c63e32376 
      KeyName: ali-key-pair
      AvailabilityZone: us-east-1d
      SecurityGroupIds:
        - !Ref MySecurityGroup
      Tags:
        - Key: Name
          Value: WebServer2_viaCode
      UserData:
        Fn::Base64: 
          !Sub |
            #!/bin/bash
            yum update -y
            yum install -y httpd.x86_64
            systemctl start httpd.service
            systemctl enable httpd.service
            echo ?Hello World from $(hostname -f)? > /var/www/html/index.html

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: AliSecurityGroup
      GroupDescription: "Allow HTTP access"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyLoadBal:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties: 
     IpAddressType: ipv4
     Name: LoadBal
     Scheme: internet-facing
     SecurityGroups: 
        - sg-0ea2e16045983f3e6
     Subnets: 
        - subnet-0cda0bd60dca22ecb
        - subnet-0f3c1982a7e39b7c3

  MyTargetGroup:
    Type: AWS::ElasticLoadBalancingV2::TargetGroup
    Properties: 
     Name: AliTargetGrp
     Port: 80
     Protocol: HTTP
     ProtocolVersion: HTTP1
     Targets: 
       - Id: !Ref MyInstance1
         Port: 80
       - Id: !Ref MyInstance2
         Port: 80
     TargetType: instance          
     VpcId: vpc-0f06c8dfdd507518e

  LoadBalancerListener:
    Type: AWS::ElasticLoadBalancingV2::Listener
    Properties:
     LoadBalancerArn: !Ref MyLoadBal
     Port: 80
     Protocol: HTTP
     DefaultActions:
        - Type: forward
          TargetGroupArn: !Ref MyTargetGroup

```
