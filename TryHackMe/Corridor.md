# Corridor

Use nmap scan to know about the open ports 
```shell
┌──(grimlock㉿GRIMLOCK)-[~]
└─$ nmap -sC -A  10.10.253.162
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-22 14:51 IST
Stats: 0:00:42 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 87.07% done; ETC: 14:52 (0:00:00 remaining)
Nmap scan report for something (10.10.253.162)
Host is up (0.21s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Werkzeug httpd 2.0.3 (Python 3.10.2)
|_http-server-header: Werkzeug/2.0.3 Python/3.10.2
|_http-title: Corridor

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.81 seconds
```
Now we got to know that the http port is open, so now we visit the website

---

Viewing the website we get to know that we can access those doors but nothing much is being presented in the website. Now look carefully in the url and the value is nothing but the md5 hash value of the number of the door. 

So now we use md5 hash value of 0 to access the flag.

Flag :- flag{2477ef02448ad9156661ac40a6b8862e} 