---
layout: post
title:  "List of AWS CLI commands for Cloudwatch Service"
description: List of AWS CLI commands for Cloudwatch Service.
date:   2020-05-09 18:00:00 +0530
categories: cheatsheet
author: senthil
---

List the CloudWatch metrics based on metric name:
```
  aws cloudwatch list-metrics --metric-name <METRIC NAME> --output text
  EX:
  aws cloudwatch list-metrics --metric-name MemoryUtilization --output text
  aws cloudwatch list-metrics --metric-name CPUUtilization --output text
```
  
List the CloudWatch metrics based on Instance Name, Start/End Time, Statics(AVG,SUM,MAX,MIN,SAMPLECOUNT) and Sorting the details by Date/Time
```
  aws cloudwatch get-metric-statistics --metric-name <METRIC NAME --start-time <START TIME> --end-time <END TIME> --period <TIME in SEC> --namespace 
<NAMESPACE> --statistics <STATISTICS> --dimensions Name=InstanceId,Value=<INSTANCE ID> --region <REGION> --query 'sort_by(Datapoints,&Timestamp)[*]'
  EX:
  aws cloudwatch get-metric-statistics --metric-name MemoryUtilization --start-time 2017-06-06T00:00:01 --end-time 2017-09-06T23:59:59 --period 86400 --namespace 
System/Linux --statistics Maximum --dimensions Name=InstanceId,Value=i-0e9343fab1a3d7a3c --region us-east-1 --query 'sort_by(Datapoints,&Timestamp)[*]'
```

Get the Alarm names with Status:
```
  aws cloudwatch describe-alarms --query "MetricAlarms[*].[AlarmName,StateValue]" --output text
```
