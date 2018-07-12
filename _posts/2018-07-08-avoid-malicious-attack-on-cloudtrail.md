---
layout: post
title:  "Avoid malicious attack on CloudTrail using CloudWatch and Lambda"
description: As we all know that CloudTrail(API Tracker) is very important service provided by AWS. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. 
date:   2018-07-08 19:00:00 +0530
categories: automation
author: senthil
---

As we all know that CloudTrail(API Tracker) is very important service provided by AWS. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. This event history simplifies security analysis, resource change tracking, and troubleshooting.

With the help of CloudTrail logs, we can identify the actions which are made by a user. If a user disables CloudTrail logs accidentally or with malicious intent then audit logging events will not captured and hence you fail to have proper governance in place and It will put us in the complex situation.

We should have the work around or monitoring to resolve this issue. We can achive this by using the CloudWatch events and Lambda function. Cloudwatch event rule will trigger the corresponding target which is Lambda function based on the event pattern. This Lambda function will automatically enable the CloudTrail and will notify us through the SNS with information like who disable the CloudTrail, Source IP and etc.

We have illustrated below the detailed steps which architecure diagram on how to configure this event.

Architecture:

![]({{site.baseurl}}/images/cloudwatchrulecloudtrailarchitecture.PNG)

Before implementing this work around please make sure you have already enabled the CloudTrail on your AWS Account.

To create a CloudWatch Event Rule, Please follow the below steps:

Go to CloudWatch Console --> Click Rules from Events --> Then Click Create Rule

Here you need to choose the event pattern and Service Name, Event Type, Specific Operations Values as you see in the below screenshot. For the target choose the Lambda function which will enable the CloudTrail. 

![]({{site.baseurl}}/images/cloudwatchrulecloudtrail1.PNG)

Event Pattern will be like below:
```
{
  "source": [
    "aws.cloudtrail"
  ],
  "detail-type": [
    "AWS API Call via CloudTrail"
  ],
  "detail": {
    "eventSource": [
      "cloudtrail.amazonaws.com"
    ],
    "eventName": [
      "StopLogging"
    ]
  }
}
```
In this screen, Enter the values for Rule name, Description and Click Create Rule.
![]({{site.baseurl}}/images/cloudwatchrulecloudtrail2.PNG)

Your CloudWatch Rule will be look like below.

![]({{site.baseurl}}/images/cloudwatchrulecloudtrail3.PNG)

Please see <a href='https://docs.aws.amazon.com/lambda/latest/dg/get-started-create-function.html'>how to create a Lambda function</a>

Lambda Function Code(Python):
```
#!/usr/bin/python
# -*- coding: utf-8 -*-
import json
import boto3
import sys
print("Loading function")


# Function to define Lambda Handler

def lambda_handler(event, context):
    try:
        client = boto3.client('cloudtrail')
        sns = boto3.client('sns')
        print("EveNt",event);
        if event['detail']['eventName'] == 'StopLogging':
            response = client.start_logging(Name=event['detail'
                    ]['requestParameters']['name'])
            iamuser = event['detail']['userIdentity']['arn'].split("/")[-1]
            print("IAM USER", iamuser)
            message = """Hi Team,\n
                    Cloudtrail has been disabled by %s IAM User.
                    Please investigate on this""" %(iamuser)
            response = sns.publish(
            TopicArn='arn:aws:sns:us-west-2:<AWSACCOUNTID>:<SNS Topic>',
            Subject="AWS Health Notification",
            Message=message)
        
    except Exception, e:

        sys.exit()
```
NOTE: Please change the ARN value for SNS Topic to receive the email notification.

That's it. We have deployed the solution now. You will be notified and CloudTrail will be enabled automatically if someone disabled the CloudTrail.

