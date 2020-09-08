# Tryhackme Room Address

***https://tryhackme.com/room/sudovulnsbof***

A tutorial room exploring CVE-2019-18634 in the Unix Sudo Program. Room Two in the SudoVulns Series


## [Task 1] Deploy! 

## [Task 2] Buffer Overflow 

SSH into that machine you deployed earlier, using port 4444.

The credentials are:

Username: tryhackme
Password: tryhackme

If you're using Linux, the command will look like this:

ssh -p 4444 tryhackme@10.10.219.194

you can download the exploit at ***https://raw.githubusercontent.com/saleemrashid/sudo-cve-2019-18634/master/exploit.c***

compile it on your machine ***gcc -o exploit exploit.c***

make an http server

python3 -m http.server 

use wget to upload to the target

exploit it 

we are root now

