---
layout: post
title:  "How to add input step to jenkins pipline"
description: This blog explains How to add input step to jenkins pipline script groovy script. 
date:   2020-04-04 11:00:00 +0530
categories: jenkins
author: senthil
---

What is Input Step in Jenkins pipeline?

This step pauses Pipeline execution and allows the user to interact and control the flow of the build. Only a basic "process" or "abort" option is provided in the stage view.

We can use below options in the Input Step.

* message:
This parameter gives a prompt which will be shown to a human:

    Ready to go?
    Proceed or Abort
    
If you click "Proceed" the build will proceed to the next step, if you click "Abort" the build will be aborted.

Type: String

* id (optional)
Every input step has an unique ID. It is used in the generated URL to proceed or abort.

A specific ID could be used, for example, to mechanically respond to the input from some external process/tool.

Type: String

* ok (optional)

Type: String

* parameters (optional)
Request that the submitter specify one or more parameter values when approving. If just one parameter is listed, its value will become the value of the input step. If multiple parameters are listed, the return value will be a map keyed by the parameter names. If parameters are not requested, the step returns nothing if approved.

Sample Code for Input Step:
```
node("master") {
    def feedback = input(submitterParameter: 'approver', id: "approve", message: "Provide your approval to proceed")
    echo "It was ${feedback} who approved."
}
```

Note: ${userInput.approver} is prints the user name who approves this job

Get the users remarks/input while approving the job:
```
node("master") {
    def userInput = input(submitterParameter: "approver", id: "approve", message: "Provide your approval to proceed",
    parameters: [string(defaultValue: "approved", description: 'Please provide the message why your are approving', name: 'remarks')])
    echo "Remarks: ${userInput['remarks']}"
    echo "It was ${userInput.approver} who approved this job"
}
```

