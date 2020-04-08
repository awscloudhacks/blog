---
layout: post
title:  "How to build the jenkins job periodically with parameters"
description:  In this blog, we are going to see about how to build the jenkins job periodically with parameters. 
date:   2020-04-07 11:00:00 +0530
categories: jenkins
author: senthil
---

In this blog, we are going to see about How to use “Jenkins build periodically with parameters”? Using “Parameterized Scheduler” Plugin. The default, Parameterized Scheduler plugin will not be installed in Jenkins.

Steps:

* Setup the Parameterized Scheduler plugin

In “Manage Jenkins” –> In "Available" tab –> Select "Parameterized Scheduler" –> click "Install without restart".

* Configure job

In this example, I using two parameters: NAME and CITY.


In “Build Triggers” tab, select “Build periodically with parameters”

Jenkins setting automation run job with parameters every fifteen minutes as the picture below

```
# every fifteen minutes auto run job
H/15 * * * * % NAME=Foo; CITY=NEWYORK
```

In the "Execute shell" basic.

```
echo "My name is ${NAME}"
echo "My city is ${CITY}"
```


