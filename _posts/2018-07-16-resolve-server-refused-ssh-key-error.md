---
layout: post
title:  "How to resolve the issue when receive an error that server refused our key"
description: While we are using SSH Private key to login into EC2 instance, there might be situation that we will receive an error like server refused our key. How can we resolve this issue.
date:   2018-07-16 14:10:00 +0530
categories: know-how-to
author: senthil
---

While we are using SSH Private key to login into EC2 instance, there might be situation that we will receive an error like server refused our key. How can we resolve this issue?

There are many situation which will give us this kind of an error.

* You're using an SSH private key but the corresponding public key is not in the authorized_keys file.
* Your instance was launched without a key, or it was launched with an incorrect key.
* Your authorized_keys file or .ssh folder isn't named correctly.
* Your authorized_keys file or .ssh folder was deleted.
* You don't have permissions for your authorized_keys file.
* You don't have permissions for the .ssh folder.

To connect to your EC2 instance after receiving the error "Server refused our key," you can update the instance's user data to append the specified SSH public key to the authorized_keys file, which sets the appropriate ownership and file permissions for the SSH directory and files contained in it.

Follow below steps to update your instance's user data to deploy the required SSH Public on authorized_keys file:

* Select the problematic instance from EC2 Console

* Go to Actions --> Instance State --> Stop.[Note: You can't change the SSH key using this procedure if your instance's root device is an instance store volume.]

* Generate the public key by using below linux command.

```
# ssh-keygen -y -f /path/to/keypair.pem
```

* From EC2 Console, Go to Actions --> Instance Settings --> View/Change User Data.

* In the View/Change User Data dialog box, paste below snippet with generated Public key.

```

#cloud-config
ssh_deletekeys: false
ssh_authorized_keys:
  - ssh-rsa ENTER YOUR PUBLIC KEY HERE ...
cloud_final_modules:
  - [ssh, always]

```

* Choose Save.

* Choose Actions, choose Instance State, and then choose Start.

After the instance starts, you can log in with the user name. 

Note:
* The procedure doesn't correct the issue if permissions to the home directory are broken. You must manually correct the home directory permissions.
* The procedure applies to all distributions that support cloud-init directives. Cloud-init must be installed and configured for these instructions to be successful. For more information about the cloud-init SSH module, see Configure ssh and ssh keys.
* You must stop your instance at the beginning of this procedure. Any data on ephemeral volumes is lost.
* You can't change the SSH key using this procedure if your instance's root device is an instance store volume.