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

To do a restrat of ssh service.

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

Passwords are not well perfect. Public Key authentication is a way to logging to your SSH server using keys. This eliminates the need of sharing passwords between users. This has a lot of advantages. For example if an attacker got the public key and also got the password, still he can't gain access to the server without private key.

To use Public Key authentication follow the below steps.

 * Generate a SSH key pair. To do this issue the below command on your PC.
 
 ~~~
 ssh-keygen
 ~~~
 
 Don't leave passphrase empty, give a super password.

* Next step is to move the public key to the server, for that use **ssh-copy-id** or you can do that manually.

~~~
ssh-copy-id -i '/home/masscan/.ssh/id_rsa.pub' root@135.181.XX.XX
~~~

If you are doing manually, just copy the content from file **id_rsa.pub** and put that inside server's **~/.ssh/authorized_keys**.

Now you can ssh into your server without password.

~~~
ssh root@135.181.XX.XX
OR
ssh -i </path/to/privatekey> root@135.181.XX.XX
~~~


### 3 - Disable password based authentication

Good passwords are little hard to remember, so lazy users will come up with easy and bad passwords. There is also chance to use a password more that one place, this increase the risk. Public key based authentication is much better than password based auth.

To disable password based authentication edit the file **"/etc/ssh/sshd_config"**. 

~~~
#PasswordAuthentication yes
PasswordAuthentication no
~~~
if you need password based auth change the value **no** to **yes** and use tool **fail2ban** to protect SSH server from bruteforce attacks.

Before doing this don't forget to enable public key based authentication.


### 4 - Disable root login

Another best practice is to disallow login attempts from root users. It is a best idea to create a user and add that user to **sudo** (previleged group) group.
This will help us to improve security because it will be hard to guess the username for SSH access. Attackers can't be successfull with bruteforcing cause most of the attacks will be against root user but we blocked root user from ssh.

To disable root login edit the file **"/etc/ssh/sshd_config"**. 

~~~
PermitRootLogin prohibit-password
OR
PermitRootLogin no
~~~
* **prohibit-password** - this will allow root user to log in to ssh only through ssh keys.

### 5 - Block unwanted connections

Using **TCP wrappers** we can filter out unwanted connections to our ssh server. TCP wrappers uses **hosts.deny**, **hosts.allow**. This service is mostly installed by default, if not installed you can install.

When a connection coming to our server, the server checks these two files.

To allow ssh only from 192.168.x.x do the following steps mentioned below.

* Edit the **/etc/hosts.deny** file and add the below line at the end
~~~
sshd: ALL
~~~
* This will block all ssh connections.

* After that edit the **/etc/hosts.allow** file and add the below line at the end
~~~
sshd : 192.168.x.x,LOCAL
~~~
* This will allow ssh connections only from 192.168.x.x and localhost.

Now only the specified clients can do a SSH and all other connections will be dropped.

These are the some best practices with SSH. There are many more to do like disable empty password login, set a idle timeout value, disable forawarding etc.

I hope this will help.
