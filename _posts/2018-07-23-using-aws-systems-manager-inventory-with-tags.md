---
layout: post
title:  "Using AWS Systems Manager Inventory with Tags"
description: AWS Systems Manager Inventory now supports Tags. Tags enable you to categorize your AWS resources in several ways, for example, by purpose, function, owner, or environment. . 
date:   2018-07-23 14:10:00 +0530
categories: awsblog
author: senthil
---

AWS Systems Manager Inventory now supports Tags. Tags enable you to categorize your AWS resources in several ways, for example, by purpose, function, owner, or environment. Consequently, when you use AWS Systems Manager Inventory to collect metadata from an instance, it also collects the tag information attached to the instance, making the tag information available as part of the inventory metadata of the instance. This allows customers to filter or query inventory metadata based on the instance categorization represented by tags, for example, you can query inventory only for instances in a production environment.

Further, when you use Resource Data Sync to sync inventory data to an Amazon S3 bucket, the tag data is also synchronized to the S3 bucket. This enables additional analytics and query capabilities on inventory data in the S3 bucket. When you use Amazon Athena and Amazon QuickSight, now tags can come in handy when you need to analyze or visualize the categorized inventory data.

<a href='https://aws.amazon.com/blogs/mt/using-aws-systems-manager-inventory-with-tags/'>Click here.</a> To learn more about AWS Systems Manager Inventory with Tags