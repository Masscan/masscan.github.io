---
layout: post
title: Iptables - Firewall Tool For Linux
tags: [Linux Administration]
comments: true
---

Iptables is a administration tool for packet filtering and NAT or simply it is a firewall tool for linux.

Iptables will monitor traffic using tables where rules are defined. 

There is several diffrent tables, each table contains a number of built in chains and may also contain user defined chains.

Each chain is a set of rules that can match against a set of packets. Each rule specifies what to do with that packet that matches, this is called **target**.

If  the packet  does  not  match, the next rule in the chain is examined. If it does match, then the next rule is specified by the value of the target, which  can  be the name of a user-defined chain, one of the targets described in iptables-extensions(8), or one of the special values **ACCEPT,DROP or RETURN**.

Following are the possible special values that you can specify in the target.

~~~
ACCEPT – Let the packet to pass through.

DROP   – Drop the packet.

RETURN – Stop traversing this chain  and  resume  at the  next rule in the previous (calling) chain.
~~~

Iptable currently  have 5 tables, **filter, nat, mangle, raw, security**.
	
In this blog we will discuss about filter table. which is the default table.

Filter table consists of 3 chains.

~~~
INPUT   - Filter incoming traffic coming to server.

FORWARD - For packets routed through the server.

OUTPUT  - For controlling locally generated packets before routing.
~~~

#### 1 - Installing Iptables

To install Iptables run the below command on the terminal.

~~~
sudo apt-get update

sudo apt-get install iptables
~~~

Run the below command to check if iptables is installed successfully.

~~~
sudo iptables -L -v --line-number
~~~

![masscam]((https://raw.githubusercontent.com/Masscan/masscan.github.io/master/assets/img/iptable-basic.png))

This command will list all rules from **filter table** (default table). 

* **-L** is to list all the rules(from all the chains).

* **-v** is to enable verbose mode.

* **--line-number** is to list rules with line number.

