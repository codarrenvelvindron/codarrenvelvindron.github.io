---
layout: post
title: 'Day 38: Basic understanding iptables firewall'
published: true
---
## iptables
![iptables no logo](https://github.com/codarrenvelvindron/codarrenvelvindron.github.io/raw/master/images/iptables_featured.png)

This is a guide on the terms used in iptables.

I believe the major obstacle to using iptables is the lack of understanding of the terms.


## Listing rules

```
codax@gaming:~$ sudo iptables -L
[sudo] password for codax: 
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```
Listing rules is the most basic command to seeing the rules that are actually defined on iptables.

This is the first argument that you will be using most of the time.

## Chain

As we see above, when we list rules, there are 3 chains that can be seen.

The INPUT, FORWARD and OUTPUT chains.

First of all, we'll refer to the machine running iptables as the HOST.

### Chains - INPUT
The INPUT chain, relates to all packets that are *received* by the HOST.

Let's say we do not wish to allow other hosts to send a ping request to my HOST

```
#e.g.
#For simplicity, we want to block IP 192.168.1.188 from sending requests to my HOST
iptables -I INPUT -s 192.168.1.188 -p icmp -j DROP
```


**Why do we make use of the INPUT chain ?**

Because it refers to all packets that are send to my HOST OR all packets that are received by my HOST. (basically the same thing)

Let's translate the command above in English.

**iptables** **I**nsert a new rule to the **INPUT** chain; taking as **s**ource the IP **192.168.1.188**; having **p**rotocol **icmp**; **j**ump to the **DROP** target

More of the available [target](http://www.faqs.org/docs/iptables/targets.html) options here

### Chains - OUTPUT
The OUTPUT chain, relates to all packets that are *emitted/sent* by my HOST.

This allows me to do stuff from my HOST to other servers.

```
#Say, I wish to be able to connect remotely to other servers using SSH from my HOST
iptables -A OUTPUT -o eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
```

Translating the above command in English:

**iptables** **A**ppend a new rule to the **OUTPUT** chain; for **o**ut-interface **eth0**;having **p**rotocol **tcp**; **d**estination**port** **22**; **m**atching connection tracking **state** for **NEW** and **ESTABLISHED** connections; **j**ump to the **ACCEPT** target.

### Chains - FORWARD
The FORWARD chain is for packets that are neither emitted by the host nor directed to the host. They are the packets that the host is merely routing.

```
This is when the HOST is used as an actual firewall for filtering packets to a NETWORK to and from a network. 
[device on network] -->packets--> [HOST] -->packets--> [Destination]

e.g. We want to forward requests to our teamspeak server (running on port 8080) 
iptables -A FORWARD -p tcp -d 192.168.1.2 --dport 8080 -j ACCEPT
```

Translating to English:

**iptables** **A**ppend a new rule to the **FORWARD** chain; for packets having **p**rotocol **tcp**; to **d**estination **192.168.1.2** (our TEAMSPEAK server); running on **d**estination**port** **8080**, **j**ump to the **ACCEPT** target.

## -A v/s -I
**A**ppend adds the rules at the end of the ruleset, whereas **I**nsert add the rules at the TOP of the ruleset or at a specific position in the ruleset.

When the position of the rule is not important, you will use -A but , when position/priority is important, you will use -I.


## \ Codarren /

## Credits
[iptables questions](https://unix.stackexchange.com/questions/96548/what-is-the-difference-between-output-and-forward-chains-in-iptables)

[iptables jumps](http://www.faqs.org/docs/iptables/targets.html)

[iptable append and insert diff](https://serverfault.com/questions/472258/difference-between-iptables-a-and-i-option)

[iptables match](http://www.faqs.org/docs/iptables/matches.html)
