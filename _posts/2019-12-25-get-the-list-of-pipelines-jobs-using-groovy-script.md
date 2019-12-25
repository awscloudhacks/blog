---
layout: post
title:  "Get all pipeline Jobs in Jenkins Groovy Script using script console"
description: This blog explains how to get all pipeline Jobs in Jenkins Groovy Script using script console. 
date:   2019-12-25 11:00:00 +0530
categories: jenkins
author: senthil
---

Step-by-Step Guide

1. Go to the https://JENKINS-URL/script

2. Copy and paste below content in the text box and click "Run"

    ```
    for (job in Hudson.instance.getAllItems(org.jenkinsci.plugins.workflow.job.WorkflowJob)) {
        println job.fullName
    }
    ```

![]({{site.baseurl}}/images/jenkins-pipeline-list-using-groovy-script.PNG)

Additional scripts:

1. To print the name of all jobs including jobs inside of a folder and the folders themselves

    ```
    Jenkins.instance.getAllItems(AbstractItem.class).each {
        println(it.fullName)
    };
    ```

2. To print the name of all jobs including jobs inside of a folder, but not the folders themselves

    ```
    Jenkins.instance.getAllItems(Job.class).each{ 
        println it.name + " - " + it.class
    }
    ```

3. This script will recursively print the name of all jobs implementing the AbstractProject class, i.e.     Freestyle and Maven jobs.

    ```
    Jenkins.instance.getAllItems(AbstractProject.class).each {it ->
        println it.fullName;
    }
    ```
