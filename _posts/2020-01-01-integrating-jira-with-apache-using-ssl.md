---
layout: post
title:  "Integrating JIRA with Apache using SSL"
description: This page describes how to integrate Apache HTTP Server (also referred to as httpd) with JIRA, utilizing mod_proxy & mod_ssl so that Apache operates as a reverse-proxy over HTTPS. 
date:   2020-01-01 11:00:00 +0530
categories: know-how-to
author: senthil
---

This page describes how to integrate Apache HTTP Server (also referred to as httpd) with JIRA, utilizing mod_proxy & mod_ssl so that Apache operates as a reverse-proxy over HTTPS.

Step-by-step Guide:
1. I assume JIRA is already installed and it is running on port 8080.

2. Install Apache HTTPD Server and start the service
    ```
    apt-get install apache2 -y
    
    systemctl enable apache2 

    systemctl start apache2
    ```

3. Update Apache Configuration. Create the following file with the contents listed below.
   cat /etc/apache2/site-available/jira.conf
    ```
    <IfModule mod_ssl.c>
        <VirtualHost *:443>
            ServerName www.example.com

            SSLEngine On
            ProxyRequests Off
        
        <Proxy *>
                Require all granted
        </Proxy>
    
        ProxyPass / http://www.example.com:8080/
        ProxyPassReverse / http://www.example.com:8080/
        ErrorLog ${APACHE_LOG_DIR}/error-www.example.com.log
        CustomLog ${APACHE_LOG_DIR}/access-www.example.com combined

        SSLCertificateFile /etc/apache2/ssl/www.example.com.key
                SSLCertificateKeyFile /etc/apache2/ssl/www.example.com.pem
        SSLCertificateChainFile  /etc/apache2/ssl/www.example.com.ca

            <FilesMatch "\.(cgi|shtml|phtml|php)$">
                    SSLOptions +StdEnvVars
            </FilesMatch>
            <Directory /usr/lib/cgi-bin>
                    SSLOptions +StdEnvVars
            </Directory>


            BrowserMatch "MSIE [2-6]" \
                                nokeepalive ssl-unclean-shutdown \
                                downgrade-1.0 force-response-1.0
        </VirtualHost>
    </IfModule>
    ```

    Note: Please store the Certificate file, Chain file and Private key file in /etc/apache2/ssl directory. Also change the www.example.com domain name to jira domain name.

4. Comment the default connector and add below connector in the server.xml file( which is available at conf directory of jira) Ex: /opt/atlassian/jira/conf
    ```
    <Connector port="8080"
        relaxedPathChars="[]|" relaxedQueryChars="[]|{}^&#x5c;&#x60;&quot;&lt;&gt;"
        maxThreads="150"
        minSpareThreads="25"
        connectionTimeout="20000"

        scheme="https"
        proxyName="www.example.com"
        proxyPort="443"

        enableLookups="false"
        maxHttpHeaderSize="8192"
        protocol="HTTP/1.1"
        useBodyEncodingForURI="true"
        redirectPort="8443"
        acceptCount="100"
        disableUploadTimeout="true"
        bindOnInit="false"/>
    ```

   Note: Please change the www.example.com domain name to jira domain name.

5. Run the below command to enable the jira apache ssl configuration and reload the apache configuration 
    ```
    a2ensite jira
    systemctl reload apache2
    ```

   Note: jira is the name of the ssl configuration file without .conf extension

6. Restart the JIRA and Apache service.
   For Jira from bin directory run below commands
    ```
    ./shutdown.sh
    ./startup.sh
    ```

   For Apache2:
    ```
    systemctl restart apache2
    ```

7. Access the JIRA url with HTTPS protocol. Ex: https://www.example.com

