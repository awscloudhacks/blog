---
layout: post
title:  "List of AWS CLI commands for EC2 Service"
description: In this article, we are going to see how we can manage the EC2 Service with AWS CLI..
date:   2020-05-09 18:00:00 +0530
categories: cheatsheet
author: senthil
---

Print Instance ID
```
  aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId]' --output text 
```

Print Instance ID and Instance Name
```
  aws ec2 describe-instances --query 'Reservations[].Instances[].[InstanceId,Tags[?Key==`Name`].Value[]]' --output text
```
  
Print Instance ID, Instance Name, Instance Type and Availability Zone
```
  aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,Placement.AvailabilityZone,Tags[?Key==`Name`].Value[]]' --output text
```
  
Get the particular Instance's Information using Instance ID
```
  aws ec2 describe-instances --instance-id <INSTANCE ID> --query 'Reservations[].Instances[].<INSTANCE ATTRIBUTES'
  EX:
  aws ec2 describe-instances --instance-id <INSTANCE ID> --query 'Reservations[].Instances[].InstanceType'
```

Start EC2 instance
```
  aws ec2 start-instances --instance-ids <INSTANCE ID>
```
 
Check whether security group is attached to any resource
```
  aws ec2 describe-network-interfaces --filters Name=group-id,Values=<SG-ID>
```

Ref Link: https://serverfault.com/questions/546012/how-to-determine-aws-security-group-dependencies


To view the status of instance
```
  aws ec2 describe-instances
```

To stop an AWS instance
```
  aws ec2 stop-instances --instance-ids <INSTANCE ID>
```

To terminate an AWS instance
```
  aws ec2 terminate-instances --instance-ids <INSTANCE ID>
```

To display the subset of all available ec2 images
```
  aws ec2 describe-images | grep ubuntu
```

To Reboot an AWS instance
```
  aws ec2 reboot-instances --instance-ids <INSTANCE ID>
```

To Create New image
```
  aws ec2 create-image --instance-id <INSTANCE ID>
```

To Delete an image
```
  aws ec2 deregister-image --image-id <IMAGE ID>
```

To Delete an snapshot of image
```
  aws ec2 delete-snapshot --snapshot-id <SNAPSHOT ID>
```

To get an system log
```
  aws ec2 get-console-output --instance-id <INSTANCE ID>
```

Returns NATGW ID
```
  aws ec2 describe-nat-gateways --query 'NatGateways[?SubnetId==`<SUBNET-ID>`].NatGatewayId'
```
