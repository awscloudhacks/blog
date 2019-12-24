---
layout: post
title:  "Running Jenkins over SSL with Apache on Centos or Amazon linux"
description: This blog explains how to run jenkins over ssl with apache in centos or amazon linux. 
date:   2019-12-24 11:00:00 +0530
categories: Jenkins
author: senthil
---

This blog explains how to run jenkins over ssl with apache in centos or amazon linux. 

Step-by-step guide:
1. Jenkins is already installed, so I just checked it was working and I was able to login

2. Install Apache HTTPD Server
    ```
	yum install httpd mod_ssl -y
    
    systemctl enable httpd

	systemctl start httpd

    ```

3. Update Jenkins configuration to use the prefix /jenkins

Update /etc/sysconfig/jenkins line to match below
    ```
    JENKINS_ARGS="–prefix=/jenkins"
    ```
4. Update Apache Configuration

Create the following file with the contents listed below

# cat /etc/httpd/conf.d/jenkins.conf
    ```
        <VirtualHost *:443>
        NameVirtualHost *:80
        NameVirtualHost *:443

        <VirtualHost *:80>
            ServerAdmin jenkins.awscloudhacks.com 
            Redirect permanent / https://jenkins.awscloudhacks.com/
        </VirtualHost>

        <VirtualHost *:443>
            ServerName jenkins.awscloudhacks.com

            SSLEngine On
            ProxyRequests Off
            ProxyPreserveHost On

            ProxyPass / http://localhost:8080/ nocanon
            ProxyPassReverse / http://localhost:8080/
            ProxyPassReverse / http://jenkins.awscloudhacks.com/
            AllowEncodedSlashes NoDecode

            <Proxy http://localhost:8080/* >
                Order deny,allow
                Allow from all
            </Proxy>
            ErrorLog /var/log/httpd/error-jenkins.awscloudhacks.com.log
            CustomLog /var/log/httpd/access-jenkins.awscloudhacks.com.log combined

            SSLCertificateFile /etc/httpd/ssl/jenkins-first.key
            SSLCertificateKeyFile /etc/httpd/ssl/jenkins.pem
            SSLCertificateChainFile  /etc/httpd/ssl/jenkins-second.ca

            RequestHeader set X-Forwarded-Proto "https"
            RequestHeader set X-Forwarded-Port "443"
        </VirtualHost>
    ```


5. Add below line in the httpd.conf file
    ```
    LoadModule ssl_module modules/mod_ssl.so
    ```

6. Update SELinux Configuration. Only applicable if in Enforcing Mode
    ```
    setsebool -P httpd_can_network_connect true
    ```

7. Restart Services
    ```
    systemctl restart httpd

    systemctl restart jenkins
    ```

8. Access the jenkins url with https protocol.
    ```
    https://<Jenkins-URL>
    ```


