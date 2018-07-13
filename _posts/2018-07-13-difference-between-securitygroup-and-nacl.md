---
layout: post
title:  "Difference between Security Group and Network ACL in AWS"
description: AWS secures their environment which help of two type firewall which are Security group and Network ACL.
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
| It is the firewall of EC2 Instance  | It is the firewall of the Subnets |
| It is associated with EC2 Instance only | It is associate with Subnet only |
| All rules are applied | Rules are applied in their order (the rule with the lower number gets processed first) |
| It is a stateful which means any changes applied to an incoming rule will be automatically applied to the outgoing rule. Example: If you allow an incoming port 80, the outgoing port 80 will be automatically opened. |  It is a Stateless means any changes applied to an incoming rule will not be applied to the outgoing rule.
Example: If you allow an incoming port 80, you would also need to apply the rule for outgoing traffic. |
| Supports Allow rules only. You cannot deny a certain IP address | Supports Allow and Deny rules |