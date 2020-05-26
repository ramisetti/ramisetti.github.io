---
layout: how-to
title: SSH-Keys
subtitle: simplified
group: linux
date: 20/05/2020
#revised: 05/09/2019
---
# Use ssh-keys for secured logging and file transfer

1. Create RSA key pair<sup>[1](#1),[2](#2)</sup> by typing the below command in terminal
   ```sh
    ssh-keygen -t rsa -b 4096
   ```
   &#13;
2. Store the keys and passphrase. To use default path press enter button on keyboard  
   ```sh
    Enter file in which to save the key (/home/user/.ssh/id_rsa):
   ```
   ```sh
    Enter passphrase (empty for no passphrase):
   ```
   &#13;
3. Copying public key to remote server
   ```sh
    ssh-copy-id -i ~/.ssh/id_rsa user@remotehost    
   ```
   &#13;

**References:**

1. https://www.ssh.com/ssh/keygen/
2. https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2

