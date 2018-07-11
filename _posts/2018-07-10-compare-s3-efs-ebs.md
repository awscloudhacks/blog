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

| Features       | Amazon S3        | Amazon EFS           | Amazon EBS      |
| -------------  |:----------------:| :-------------------:| ---------------:| 
| Storage Size   | No limit on number of objects | Maximum storage size of 16 TB | No limitation on the size of the file system |
| Object/File Size Limitation     | Individual Amazon S3 objects can range from a minimum of 0 bytes to a maximum of 5TB      |   No limitation on file size in EBS disk	 | Single files have a maximum size of 52TiB |
| Data Throughput and I/O | Supports multipart upload. It is recommended for capability of objects larger than 100MB •The largest size of a single object uploaded using PUT API can be of 5GB |   SSD- and HDD-backed storage types •Use of SSD backed and Provisioned IOPS is recommended for dedicated IO operations as needed | 	Default throughput of 3GB |

