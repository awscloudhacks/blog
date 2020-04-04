---
layout: post
title:  "Basic example for declarative and scripted pipeline script"
description: This blog explains the basic example for declarative and scripted pipeline script groovy script. 
date:   2020-04-04 11:00:00 +0530
categories: jenkins
author: senthil
---

What is Jenkins Pipeline?
Jenkins Pipeline is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins.

The definition of a Jenkins Pipeline is written into a text file (called a Jenkinsfile) which in turn can be committed to a project’s source control repository

Declarative versus Scripted Pipeline syntax:
A Jenkinsfile can be written using two types of syntax - Declarative and Scripted.

Declarative and Scripted Pipelines are constructed fundamentally differently. Declarative Pipeline is a more recent feature of Jenkins Pipeline which:

* provides richer syntactical features over Scripted Pipeline syntax, and
* is designed to make writing and reading Pipeline code easier.

Sample Declarative Pipeline code:
    ```
    pipeline {
        agent {
            label "master"  //Add your jenkins slave label here
        }
        stages {
            stage('Build') {
                steps {
                    // Add your build code here
                    sh 'echo "Welcome to Declarative Pipeline"'
                }
            }
        }
    }
    ```


Sample Scripted Pipeline code:
    ```
    node("master"){
        stage ('Build') {
            //Add your build code here
            sh "whoami" 
        }
        stage ('deploy') {
            //Add your deploy code here
            sh "curl http://169.254.169.254/latest/meta-data/public-ipv4"
        }
    }
