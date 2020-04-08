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

Declarative Pipeline Configuration Example:

The parameterized cron trigger can be specified using the key parameterizedSpecification under the parameterizedCron under the triggers directive. The built in cron trigger is still available and is independent of parameterizedCron
```
pipeline {
    agent any
    parameters {
      string(name: 'PLANET', defaultValue: 'Earth', description: 'Which planet are we on?')
      string(name: 'GREETING', defaultValue: 'Hello', description: 'How shall we greet?')
    }
    triggers {
        parameterizedCron('''
            # leave spaces where you want them around the parameters. They'll be trimmed.
            # we let the build run with the default name
            */2 * * * * %GREETING=Hola;PLANET=Pluto
            */3 * * * * %PLANET=Mars
        ''')
    }
    stages {
        stage('Example') {
            steps {
                echo "${params.GREETING} ${params.PLANET}"
                script { currentBuild.description = "${params.GREETING} ${params.PLANET}" }
            }
        }
    }
}
```
