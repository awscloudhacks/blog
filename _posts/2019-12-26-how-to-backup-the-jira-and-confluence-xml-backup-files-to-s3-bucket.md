---
layout: post
title:  "Backup the Jira and confluence XML backup files to S3 bucket"
description: This blog explains how to backup the Jira and confluence XML backup files to S3 bucket. 
date:   2019-12-26 11:00:00 +0530
categories: know-how-to
author: senthil
---

Prerequisites:
1. JIRA and Confluence should be installed and enabled the automated XML backup.
2. AWS CLI is installed and configured with credentials.

Step-By-Step Guide:

1. Please deploy below scripts in the required location in the server where you have installed JIRA and Confluence.
2. Create a required S3 buckets.
Use the linux cron feature to setup the cron to run the below script in the required time.

JIRA:
```
#!/bin/bash

filelocation="/var/atlassian/application-data/jira/export"
filename=`ls -t1 $filelocation | head -n 1`
echo $filename
aws s3 cp $filelocation/$filename s3://<S3-BucketName>/ >> /var/log/jira-backup-cron.log
```

Note: Please get the filelocation from your jira configuration and update the script as required

CONFLUENCE:
```
#!/bin/bash

filelocation="/var/atlassian/application-data/confluence/backups"
filename=`ls -t1 $filelocation | head -n 1`
echo $filename
aws s3 cp $filelocation/$filename s3://<S3-BucketName>/  >> /var/log/confluence-backup-cron.log
```

Note: Please get the filelocation from your configuration configuration and update the script as required

Crontab Entry:

Please use "crontab -e" command to add below entries. 

```
00 3 * * * /opt/script/confluence-backup.sh >> /var/log/confluence-backup-cron.log
00 3 * * * /opt/script/jira-backup.sh >> /var/log/jira-backup-cron.log
```
