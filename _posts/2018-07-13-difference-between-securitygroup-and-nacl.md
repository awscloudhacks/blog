---
layout: post
title:  "Difference between Security Group and Network ACL in AWS"
description: AWS provides the three different of storage services which are S3, EFS and EBS. All these services are great, but only if you use them in accordance with their purpose.
date:   2018-07-13 10:30:00 +0530
categories: awsgeneral
author: senthil
---

AWS provides two type of firewall to secure the cloud environment.

Security group act as a firewall at the EC2 Server level which controlls both In-bound and Out-bound traffic to the instances.

Network Access Control List is NACL which act as a firewall at subnet level and controlls both In-bound and Out-bound traffic at Subnet level.

Following table summarizes basic difference betweek Security Group and NACL.

| Security Group| Network NACL     | 
|:-------------:|:----------------:| 
| Associated with Instance and Operates at server level  | Associated with Subnets and Operates at subnet level