# Task No. 3
## :memo: Created my own VPC and Network Load Balancer for multiple web servers
:heavy_check_mark: **WebServer1 in PrivateSubnet1 of my own VPC:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/49092eb3-820d-4755-b9e8-393cbff8890d)


#  
:heavy_check_mark: **PrivateSubnet1 Routes:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/5f50737a-8e31-448a-ad50-76e26711c8d4)


#
:heavy_check_mark: **WebServer2 in PrivateSubnet2 of my own VPC:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/54b60dc1-ee5f-4163-8737-ad264e42cf7c)


#
:heavy_check_mark: **PrivateSubnet2 Routes:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/9bdc5099-5468-46b2-9e8b-8ec9a731da6c)


#
:heavy_check_mark: **Load Balancer routing:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/a5c81353-3f21-4b5f-be09-726481a7bfc1)



#
:heavy_check_mark: **Load Balancer routing:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/251d99e7-cfc8-42a2-b2aa-62f818cb6a0e)


#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 192.168.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
      - Key: Name
        Value: Ali's New VPC
     
  InternetGateway:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
      - Key: Name
        Value: Ali's IG

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref VPC
      InternetGatewayId: !Ref InternetGateway

  NATGateway:
    Type: AWS::EC2::NatGateway
    Properties: 
      AllocationId: !GetAtt ElasticIPAddress.AllocationId
      SubnetId: !Ref PublicSubnet1
      Tags: 
      - Key: Name
        Value: Ali's NAT

  ElasticIPAddress:
    Type: AWS::EC2::EIP
    Properties:
      Domain: VPC

  PublicSubnet1:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 192.168.1.0/24
      AvailabilityZone: us-east-1a  
      MapPublicIpOnLaunch: true
      Tags:
      - Key: Name
        Value: Public Subnet 1

  PrivateSubnet1:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 192.168.2.0/24
      AvailabilityZone: us-east-1a  
      MapPublicIpOnLaunch: false
      Tags:
      - Key: Name
        Value: Private Subnet 1

  PublicSubnet2:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 192.168.3.0/24
      AvailabilityZone: us-east-1b  
      MapPublicIpOnLaunch: true
      Tags:
      - Key: Name
        Value: Public Subnet 2

  PrivateSubnet2:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 192.168.4.0/24
      AvailabilityZone: us-east-1b  
      MapPublicIpOnLaunch: false
      Tags:
      - Key: Name
        Value: Private Subnet 2
        
  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Public RT

  PublicRoute1: 
    Type: AWS::EC2::Route
    DependsOn: AttachGateway
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway
      
  PrivateRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC
      Tags:
      - Key: Name
        Value: Private RT

  PrivateRoute1:  
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PrivateRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      NatGatewayId: !Ref NATGateway
  
  PublicSubnet1Association:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet1
      RouteTableId: !Ref PublicRouteTable

  PrivateSubnet1Association:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet1
      RouteTableId: !Ref PrivateRouteTable

  PublicSubnet2Association:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet2
      RouteTableId: !Ref PublicRouteTable
  
  PrivateSubnet2Association:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet2
      RouteTableId: !Ref PrivateRouteTable
  
  Instance1:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-026b57f3c383c2eec
      KeyName: ali-key-pair
      AvailabilityZone: us-east-1a
      SubnetId: !Ref PrivateSubnet1
      SecurityGroupIds:
        - !Ref SecurityGroup
      Tags:
        - Key: Name
          Value: WebServer1
      UserData:
        Fn::Base64: 
          !Sub |
            #!/bin/bash
            yum update -y
            yum install -y httpd.x86_64
            systemctl start httpd.service
            systemctl enable httpd.service
            echo ?Hello World from $(hostname -f)? > /var/www/html/index.html

  Instance2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-026b57f3c383c2eec
      KeyName: ali-key-pair
      AvailabilityZone: us-east-1b
      SubnetId: !Ref PrivateSubnet2
      SecurityGroupIds:
        - !Ref SecurityGroup
      Tags:
        - Key: Name
          Value: WebServer2
      UserData:
        Fn::Base64: 
          !Sub |
            #!/bin/bash
            yum update -y
            yum install -y httpd.x86_64
            systemctl start httpd.service
            systemctl enable httpd.service
            echo ?Hello World from $(hostname -f)? > /var/www/html/index.html
  
  SecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: SG_webserver
      GroupDescription: "Allow ssh access"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
      VpcId: !Ref VPC
      
  LoadBalancer:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties: 
     IpAddressType: ipv4
     Name: NetworkLoadBalancer
     Type: network
     Subnets: 
        - !Ref PublicSubnet1
        - !Ref PublicSubnet2

  TargetGroup:
    Type: AWS::ElasticLoadBalancingV2::TargetGroup
    Properties: 
     Name: AliTargetGrp
     Port: 22
     Protocol: TCP
     Targets: 
       - Id: !Ref Instance1
         Port: 22
       - Id: !Ref Instance2
         Port: 22     
     VpcId: !Ref VPC

  Listener:
    Type: AWS::ElasticLoadBalancingV2::Listener
    Properties:
     LoadBalancerArn: !Ref LoadBalancer
     Port: 22
     Protocol: TCP
     DefaultActions:
        - Type: forward
          TargetGroupArn: !Ref TargetGroup
```
