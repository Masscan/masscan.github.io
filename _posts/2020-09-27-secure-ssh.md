---
layout: post
title: Make your SSH More Secure!
tags: [Linux Administration]
comments: true
---

**Secure Shell** (SSH) is a network protocol for operating network services securely over an unsecured network. 

SSH is typically used to login into a remote machine and execute commands, but it also supports tunneling, forwarding TCP ports, and X11 connection.

In default, an SSH server will listen for incoming connections on port 22(TCP).

Here I will discuss some methods that you can do for SSH security.

**NB: after every change made to the sshd config file, you need to issue a restart of sshd service**

To restart the ssh service, execute the below command.

~~~
sudo systemctl restart sshd
~~~


### 1 - Change default port

Changing your default SSH port will help against some automated attacks from bots and script kiddies.

To change the SSH port, edit the file **"/etc/ssh/sshd_config"**. 

If the below line is commented, first uncomment that.

~~~
#Port 22
Port 4444
~~~

Changed to port 4444 from port 22.


### 2 - Use SSH public key authentication

Passwords are not well perfect. Public Key authentication is a way to login to your SSH server using keys. This eliminates the need of sharing passwords between users. This has a lot of advantages. For example, if an attacker got the public key and also got the password, still he can't gain access to the server without a private key.

To use Public Key authentication follow the below steps.

 * Generate an SSH key pair. To do this, issue the below command on your PC.
 
 ~~~
 ssh-keygen
 ~~~
 
 Don't leave the passphrase empty, give a super password.

* Next step is to move the public key to the server, for that use **ssh-copy-id** or you can do that manually.

~~~
ssh-copy-id -i '/home/masscan/.ssh/id_rsa.pub' root@135.181.XX.XX
~~~

If you are doing manually, just copy the content from file **id_rsa.pub** and paste that inside server **"~/.ssh/authorized_keys"**.

Now you can ssh into your server without a password.

~~~
ssh root@135.181.XX.XX
OR
ssh -i </path/to/privatekey> root@135.181.XX.XX
~~~


### 3 - Disable password-based authentication

Strong passwords are a little hard to remember. Normally users will come up with easy and guessable passwords. There is also a chance to use a password more than one place or platform, this increases the risk. Public key-based authentication is much better than password-based auth.

To disable password based authentication, edit the file **"/etc/ssh/sshd_config"**. 

~~~
#PasswordAuthentication yes
PasswordAuthentication no
~~~

if you need password-based auth, change the value **no** to **yes** and use tool **fail2ban** to protect the SSH server from brute force attacks.

Before doing this don't forget to enable public key-based authentication.


### 4 - Disable root login

Another best practice is to disallow login attempts from **root** user. It is the best idea to create a user and add that user to **sudo** (privileged group) group.
This will help us to improve security, because it will be hard to guess the username for SSH access. Attackers can't be successful with brute-forcing cause most of the attacks will be against the root user, but we blocked the root user from accessing the ssh service.

To disable root login, edit the file **"/etc/ssh/sshd_config"**. 

~~~
PermitRootLogin prohibit-password
OR
PermitRootLogin no
~~~

* **prohibit-password** - this will allow root user to log in to ssh only through ssh keys.


### 5 - Block unwanted connections

Using **TCP wrappers** we can filter out unwanted connections to our ssh server. TCP wrappers use **hosts.deny**, **hosts.allow** files. This service is mostly installed by default, if not installed, you can install.

When a connection coming to our server, the server checks these two files.

To allow ssh only from 192.168.x.x do the following steps mentioned below.

* Edit the **/etc/hosts.deny** file and add the below line at the end.

~~~
sshd: ALL
~~~

* This will block all ssh connections.

* After that edit the **/etc/hosts.allow** file and add the below line at the end.

~~~
sshd : 192.168.x.x,LOCAL
~~~

* This will allow ssh connections only from 192.168.x.x and localhost.

Now only the specified clients can do an SSH and all other connections will be dropped.

These are some of the best practices that you can do with SSH. There are many more to do like disable empty password login, set an idle timeout value, disable forwarding, etc.

I hope this will help.
