---
layout: post
title:  "Running Jenkins over SSL with Apache on Centos or Amazon linux"
description: This blog explains how to run jenkins over ssl with apache in centos or amazon linux. 
date:   2019-12-24 11:00:00 +0530
categories: jenkins
author: senthil
---

This blog explains how to run jenkins over ssl with apache in centos or amazon linux. 

Step-by-step guide:
1. I assume Jenkins is already installed, so I just checked it was working and I was able to login

2. Install Apache HTTPD Server
    ```
	yum install httpd mod_ssl -y
    
    systemctl enable httpd

	systemctl start httpd

    ```

3. Update Jenkins configuration to use the prefix /jenkins. Update /etc/sysconfig/jenkins line to match below
    ```
    JENKINS_ARGS="–prefix=/jenkins"
    ```

4. Update Apache Configuration. Create the following file with the contents listed below

cat /etc/httpd/conf.d/jenkins.conf

    ```
    NameVirtualHost *:80
    NameVirtualHost *:443

    <VirtualHost *:80>
        ServerAdmin www.example.com 
        Redirect permanent / https://www.example.com/
    </VirtualHost>

    <VirtualHost *:443>
        ServerName www.example.com

        SSLEngine On
        ProxyRequests Off
        ProxyPreserveHost On

        ProxyPass / http://localhost:8080/ nocanon
        ProxyPassReverse / http://localhost:8080/
        ProxyPassReverse / http://www.example.com/
        AllowEncodedSlashes NoDecode

        <Proxy http://localhost:8080/* >
            Order deny,allow
            Allow from all
        </Proxy>
        ErrorLog /var/log/httpd/error-www.example.com.log
        CustomLog /var/log/httpd/access-www.example.com.log combined

        SSLCertificateFile /etc/httpd/ssl/jenkins.key
        SSLCertificateKeyFile /etc/httpd/ssl/jenkins.pem
        SSLCertificateChainFile  /etc/httpd/ssl/jenkins.ca

        RequestHeader set X-Forwarded-Proto "https"
        RequestHeader set X-Forwarded-Port "443"
    </VirtualHost>
    ```

Note: Please store the Certificate file, Chain file and Private key file in /etc/httpd/ssl directory. Also change the www.example.com to jenkins domain name.

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


