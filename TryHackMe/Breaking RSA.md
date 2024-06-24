# Breaking RSA

### 1. How many services are running on the box?
---

Answer : 2

---

### 2. What is the name of the hidden directory on the web server? (without leading '/')
---

Answer : development 

---
### 3. What is the length of the discovered RSA key? (in bits) 
---

Answer : 4096

---
### 4. What are the last 10 digits of n? (where 'n' is the modulus for the public-private key pair)
---

Answer : 1225222383

---
### 5. Factorize n into prime numbers p and q
---

Answer : No answer needed

---
### 6. What is the numerical difference between p and q? 
---

Answer : 1502

---
### 7. Generate the private key using p and q (take e = 65537) 
---

Answer : No answer needed

---
### 8. What is the flag?
---

Answer : breakingRSAissuperfun20220809134031

---
```shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ nmap -sC -A 10.10.207.213                        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-24 18:13 IST
Stats: 0:01:03 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 59.78% done; ETC: 18:14 (0:00:42 remaining)
Stats: 0:01:27 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 74.18% done; ETC: 18:15 (0:00:30 remaining)
Stats: 0:01:41 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 83.02% done; ETC: 18:15 (0:00:21 remaining)
Stats: 0:02:05 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 97.43% done; ETC: 18:15 (0:00:03 remaining)
Stats: 0:02:25 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 18:15 (0:00:06 remaining)
Nmap scan report for 10.10.207.213
Host is up (0.22s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE    SERVICE VERSION
22/tcp   open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ff:8c:c9:bb:9c:6f:6e:12:92:c0:96:0f:b5:58:c6:f8 (RSA)
|   256 67:ff:d4:09:ee:2c:8d:eb:94:b3:af:17:8e:dc:94:ae (ECDSA)
|_  256 81:0e:b2:0e:f6:64:76:3c:c3:39:72:c1:29:59:c3:3c (ED25519)
80/tcp   open     http    nginx 1.18.0 (Ubuntu)
|_http-title: Jack Of All Trades
|_http-server-header: nginx/1.18.0 (Ubuntu)
9010/tcp filtered sdr
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 154.27 seconds
```
```shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ gobuster dir -u 10.10.207.213 --wordlist /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.207.213
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/development          (Status: 301) [Size: 178] [--> http://10.10.207.213/development/]                                                                   
Progress: 979 / 87665 (1.12%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 999 / 87665 (1.14%)
===============================================================
Finished
===============================================================
```
```shell
┌──(venv)─(grimlock㉿GRIMLOCK)-[~/CTF]
└─$ ssh-keygen -l -f id_rsa.pub             
4096 SHA256:DIqTDIhboydTh2QU6i58JP+5aDRnLBPT8GwVun1n0Co no comment (RSA)
```
This code is used to get all the information about the public and private keys 
```shell
┌──(venv)─(grimlock㉿GRIMLOCK)-[~/CTF]
└─$ cat new.py                              
import gmpy2
from Crypto.PublicKey import RSA

def fermat_factorization(n):
    """Fermat's Factorization Method"""
    a = gmpy2.isqrt(n) + 1
    b2 = gmpy2.square(a) - n
    while not gmpy2.is_square(b2):
        a += 1
        b2 = gmpy2.square(a) - n
    p = a + gmpy2.isqrt(b2)
    q = a - gmpy2.isqrt(b2)
    return p, q

def calculate_private_key(p, q, e):
    """Calculate the private key 'd'"""
    phi = (p - 1) * (q - 1)
    d = gmpy2.invert(e, phi)
    return d

# Open the RSA key file
with open("id_rsa.pub", "r") as f:
    # Import the RSA key
    key = RSA.import_key(f.read())

# Retrieve the modulus (n) and public exponent (e)
n = key.n
e = key.e

# Print the modulus and public exponent
print("\033[96mModulus (n):\033[0m", n)
print("\033[96mPublic Exponent (e):\033[0m", e)

# Factorize n into p and q using Fermat's Factorization
p, q = fermat_factorization(n)

# Calculate private key 'd' using p, q, and public exponent 'e'
d = calculate_private_key(p, q, e)

difference = abs(p - q)
print("\033[93mp:\033[0m", p)
print("\033[93mq:\033[0m", q)
print("\033[93mDifference between q and p:\033[0m", difference)
print("\033[96mPrivate Key (d):\033[0m", d)

key_params = (n, e, d, p, q)
key = RSA.construct((n,e,int(d)))

# Export the private key to a file
with open('id_rsa', 'wb') as f:
    f.write(key.export_key('PEM'))

print("\033[92mPrivate key generated and saved as 'id_rsa'.\033[0m")

```

```shell
┌──(venv)─(grimlock㉿GRIMLOCK)-[~/CTF]
└─$ python3 new.py             
Modulus (n): 960343778775549488806716229688022562692463185460664314559819511657255292180827209174624059690060629715513180527734160798185034958883650709727032190772084959116259664047922715427522089353727952666824433207585440395813418471678775572995422248008108462980790558476993362919639516120538362516927622315187274971734081435230079153205750751020642956757117030852053008146976560531583447003355135460359928857010196241497604249151374353653491684214813678136396641706949128453526566651123162138806898116027920918258136713427376775618725136451984896300788465604914741872970173868541940675400325006679662030787570986695243903017923121105483935334289783830664260722704673471688470355268898058414366742781725580377180144541978809005281731232604162936015554289274471523038666760994260315829982230640668811250447030003462317740603204577123985618718687833015332554488836087898084147236609893032121172292368637672349405254772581742883431648376052937332995630141793928654990078967475194724151821689117026010445305375748604757116271353498403318409547515058838447618537811182917198454172161072247021099572638700461507432831248944781465511414308770376182766366160748136532693805002316728842876519091399408672222673058844554058431161474308624683491225222383
Public Exponent (e): 65537
p: 30989413979221186440875537962143588279079180657276785773483163084840787431751925008409382782024837335054414229548213487269055726656919580388980384353939415484564294377142773553463724248812140196477077493185374579859773369113593661078143295090153526634169495633688691753691720088511452131593712380121967802013042678209312444897975134224456911144218687330712554564836016616829044029963400114373142702236623994027926718855592051277298418373056707389464234977873660836337340136755093657804153998347162906059312569124331219753644648657722107663012261197728061352359157767204739644300066112274629356310784052940617408518123
q: 30989413979221186440875537962143588279079180657276785773483163084840787431751925008409382782024837335054414229548213487269055726656919580388980384353939415484564294377142773553463724248812140196477077493185374579859773369113593661078143295090153526634169495633688691753691720088511452131593712380121967802013042678209312444897975134224456911144218687330712554564836016616829044029963400114373142702236623994027926718855592051277298418373056707389464234977873660836337340136755093657804153998347162906059312569124331219753644648657722107663012261197728061352359157767204739644300066112274629356310784052940617408516621
Difference between q and p: 1502
Private Key (d): 355800651425135681183390707128108943722411746130741264046675581161020732554780741656912077045552316250703809090954928640931547581315194819458963160110120922414848114847301699090523885005384262610482079782617179276603225871047383152843460146236246419421648466520894698339133117031948242676251882103774390399143617061031502081556812701001453097227818768422703191949841125871910183204731160170789011284878230235436521793393724471383499879219675769700014447675151926209318073223612943831612223524959548771170877787184798441930486061891613062347672989812669710963541034708669406007894705151093105784054091751650933323300985495149924416659229876914076648270045676997650015615996341370380597228469755128170040353433374999020283706184957220148628008445610267702031710365117500478110172685332610159706254466309767816421842113204954137883347698932245158527034600826313156623260808181115243968071291416762806090766691780165333622281350406073396430046330125424011836635616883856583961232702990430895725627105993091471281133235092845210740773088363495356096518772606314489088890124113092286827052036690187298835257796804255669912241916692428526192173280096699373626286648150269450291733933394106397510904039580604434322844051351031280613818868793
Private key generated and saved as 'id_rsa'.
```
```shell
┌──(venv)─(grimlock㉿GRIMLOCK)-[~/CTF]
└─$ chmod 600 id_rsa 
┌──(venv)─(grimlock㉿GRIMLOCK)-[~/CTF]
└─$ ssh -i id_rsa root@10.10.207.213 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-124-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon 24 Jun 2024 01:19:23 PM UTC

  System load:  0.0               Processes:             108
  Usage of /:   70.1% of 4.84GB   Users logged in:       0
  Memory usage: 45%               IPv4 address for eth0: 10.10.207.213
  Swap usage:   0%


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sat Aug 13 09:35:51 2022 from 10.0.0.11
root@thm:~# ls
flag  snap
root@thm:~# cat flag 
breakingRSAissuperfun20220809134031
```