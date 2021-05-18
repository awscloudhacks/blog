---
layout: post
title:  "Integrate Bitbucket with Jenkins using Webhook"
description:  This page describes how to integrate BitBucket with Jenkins using Webhook to trigger the Jenkins job whenever commit/push happens on the repository. 
date:   2021-05-15 11:00:00 +0530
categories: jenkins
author: senthil
---

This page describes how to integrate BitBucket with Jenkins using Webhook to trigger the Jenkins job whenever commit/push happens on the repository.

Steps:

* You need to install “Bitbucket Plugin” plug-ins in Jenkins to achieve this and follow the below steps to install it

Go to Manage Jenkins → Manage Plugins →Available →Type “bitbucket” in the filter and look for “Bitbucket Plugin” and install it.

![]({{site.baseurl}}/images/jenkins-plugin-manager.png)

* Go to Jenkins and create a Pipeline job. To enable the webhook, go to configure option in the job and  click the checkbox of “Build when a change is pushed to BitBucket” and provide the repository url in the “Override Repository URL” in the text box

![]({{site.baseurl}}/images/jenkin-job-configure.png)

* Then go to the Bitbucket repository, Repository Settings →> Webhooks →> Add Webhook →> Add name as anything and url as http://JenkinsHostUrl:8081/bitbucket-hook/

![]({{site.baseurl}}/images/bitbucket-webhook.png)

Once you added the webhook, simply test it by making the changes in the repository and see if you received 200 OK response. That means you have configured it properly.
