---
layout: post
title: Make your SSH more Secure!
tags: [Linux Administration]
comments: true
---

**Secure Shell** (SSH) is a network protocol for operating network services securely over an unsecured network. 

SSH is typically used to log into a remote machine and execute commands, but it also supports tunneling, forwarding TCP ports and X11 connectios.

In default an SSH server will listen for incoming connections on port 22(TCP).

Here i will discuss some methods that you can do for SSH security.

**NB: after every changes made to sshd config file, you need to issue a restart of sshd service**


### 1 - Change default port

Changing your default SSH port will help against some automated attacks from bots and script kiddies.

To change the SSH port, edit the file **"/etc/ssh/sshd_config"**. 

If the below line is commented, first uncomment that.

{: .box-note}
#Port 22
Port 4444

Changed to port 4444 from port 22.


### 2 - Disable password based authentication

 








