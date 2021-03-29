---
layout: post
title: Android Reverse Enginnering#1
tags: [Android Security]
comments: true
---

I was given two android application to find the flag from it.

The flag format is in **userid:password**.

#### Lets start..

![Crepe](https://raw.githubusercontent.com/Masscan/masscan.github.io/master/assets/img/are1.png)

* Application 1 : Backdoor1.apk

I installed the application on emulator and the application asks for a username and password. I have to find out that.

![Crepe](https://raw.githubusercontent.com/Masscan/masscan.github.io/master/assets/img/are2.png)

First, i used **apktool** to decompile the android application.

~~~
apktool d Backdoor1.apk -o Backdoor1
~~~

After that i checked the strings.xml file, and i found username and password from there.

![Crepe](https://raw.githubusercontent.com/Masscan/masscan.github.io/master/assets/img/are3.png)

This was an easy find. 

Note : **strings.xml file is an important file. most of the time some important information will be leaked there.**


* Application 2 : Backdoor2.apk

Bye..
