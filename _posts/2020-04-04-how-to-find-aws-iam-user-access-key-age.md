---
layout: post
title:  "How to find AWS IAM user access key age using AWS CLI"
description: By default there is no AWS CLI to find the IAM user key's Access key age.
date:   2020-04-04 18:00:00 +0530
categories: awsgeneral
author: senthil
---

By default there is no AWS CLI to find the IAM user key's Access key age. Finding the Access key age of the IAM user key is needed to automate the IAM key rotation or any other automation. Please find the script below to find the Access age.

```
keyCreationDate=$(aws iam list-access-keys --user-name $userName --query 'sort_by(AccessKeyMetadata, &CreateDate)[0]' --output text | awk '{print $2}' | cut -c1-10)
systemDate=$(date '+%Y-%m-%d')
start_ts=$(date -d "$keyCreationDate" '+%s')
end_ts=$(date -d "$systemDate" '+%s')
accessKeyAge=$(( ( end_ts - start_ts )/(60*60*24))) 
echo "accessKeyAge: $accessKeyAge"
```
