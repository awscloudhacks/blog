---
layout: post
title:  "Comparision of S3 vs EFS vs EBS "
description: AWS provides the three different of storage services which are S3, EFS and EBS. All these services are great, but only if you use them in accordance with their purpose.
date:   2018-07-10 15:10:00 +0530
categories: awsgeneral
author: senthil
---

AWS S3 provides the object storages which is suitable for storing Log files, snapshots and backup files. With S3, we can store massive amount of data.

AWS EFS was designed for scalable storage which provides the shared network file system.

AWS EBS is a persistent storage device that can be used as a file system for databases, application hosting and storage, and plug and play devices.

With that brief introduction, It is very important to know which is a more suitable storage service for your specific needs.

The below table provides the comparision of all three AWS storage services.

This is a simple markdown table

| Features       | Amazon S3        | Amazon EFS           | Amazon EBS       |
| -------------  |:----------------:| :-------------------:| :---------------:| 
| Storage Size   | No limit on number of objects |  No limitation on the size of the file system | Maximum storage size of 16 TB |

| Object/File Size Limitation     | Individual Amazon S3 objects can range from a minimum of 0 bytes to a maximum of 5TB      |  Single files have a maximum size of 52TiB | No limitation on file size in EBS disk |
| Data Throughput and I/O | Supports multipart upload. It is recommended for capability of objects larger than 100MB •The largest size of a single object uploaded using PUT API can be of 5GB |   Default throughput of 3GB | SSD- and HDD-backed storage types •Use of SSD backed and Provisioned IOPS is recommended for dedicated IO operations as needed |
| Performance | Highly scalable managed service supports 100 PUT/LIST/DELETE requests per second by default. Can automatically scale till 300 PUT/LIST/DELETE requests per second or more than 800 GET requests per second | Highly Scalable Managed Service. Supports up to 7000 file system operations per second |
| Data Stored | Stored data stays in the region. •Replicas are made within the region in multiple availability zones. Amazon S3 objects can be copied to other region using the cross region replication feature | Data stored stays in the region.•Replicas are made within the region | Data stored stays in the same Availability zone. Replicas are made within the AZ for higher durability |
| Data Access | Accessible over internet based on access policy configured | Can be accessed by 1 to 1000s of EC2 instances from multiple AZs, concurrently | Can be accessed only by single EC2 instance |
| File Permissions/ File System | Can be mounted as a file system, but it is not recommended•Unlike traditional file systems, bucket permissions do not pass on to the folders by default (bucket policies can be used to achieve this) |  File storage service for use with AWS EC2 •EFS can be used as network file system for on-premise servers too using AWS Direct Connect. | Supports various file systems, including ext3 and ext4 | 
| Encryption Mechanisms | Server Side Encryption with Amazon S3-Managed Keys (SSE-Amazon S3),AWS KMS-Managed Keys (SSE-KMS), and with Customer-Provided Keys (SSE-C) •Client Side Encryption using an AWS KMS–Managed Customer Master Key (CMK) and using a client-side master key | Uses an AWS KMS–Managed Customer Master Key (CMK) and AES 256-bit Encryption standards | Uses an AWS KMS–Managed Customer Master Key (CMK) and AES 256-bit Encryption standards |
| Availability | 99.99% available | Highly available (No public SLA) | 	99.99% available
| AZ Failures | Can withstand up to two concurrent AZ failures | Every file system object is redundantly stored across multiple Availability Zones so it can survive one AZ failure. | Cannot withstand AZ failure without point-in time EBS Snapshots |

