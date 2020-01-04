---
layout: post
title:  "How to SSH with different port"
description: By default, SSH service will run on Port number 22 on most of the linux servers. For some reason, if you would like to change the port number for SSH service, you can change it in /etc/ssh/sshd_config file and restart the SSH service. 
date:   2020-01-04 11:00:00 +0530
categories: linux
author: senthil
---

By default, SSH service will run on Port number 22 on most of the linux servers. For some reason, if you would like to change the port number for SSH service, you can change it in /etc/ssh/sshd_config file and restart the SSH service.

Now we will see how to use ssh command to connect linux server when we are running SSH service on different port
```
ssh username@IPAddress -p 26
```

Note: Here 26 is the port number where SSH service is running.

We need to use different parameter for SCP command.
```
scp  filename.txt username@IPAddress:~/ -P 26 
```
