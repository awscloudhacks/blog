---
layout: post
title:  "Clone the repository in the jenkins pipeline job"
description:  This page describes how to clone the repository in the jenkins pipeline job. 
date:   2021-05-18 11:00:00 +0530
categories: jenkins
author: senthil
---

Step-By-Step Guide.

* Add the git/bitbucket credentials(username/password) in the jenkins credentials sections

* Create a jenkins pipeline job and paste the below pipeline code 

```
node("master"){
    echo "Example for clone the repository"
    stage('checkout') {
        git credentialsId: '<Credential ID>', url: '<Repository URL>', branch: 'master'
    }
}
```
