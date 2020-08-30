# Tryhackme Room Address  

**https://tryhackme.com/room/crackthehash**  

Cracking hashes challenges  

------------------------------------------

## [Task 1] Level 1   

### 1 :  

**hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt --force** 

### 2 :

use **hash-identifier** to identify the hash

**hashcat -a 0 -m 100 hash /usr/share/wordlists/rockyou.txt --force**

### 3 :

**hashcat -a 0 -m 1400 hash /usr/share/wordlists/rockyou.txt --force** 

### 4 :

**hashcat -a 0 -m 3200 hash /usr/share/ wordlists/rockyou.txt --force** 

### 5 :

**hashcat -a 0 -m 900 hash /usr/share/wordlists/rockyou.txt --force**

--------------------------------------

## [Task 2] Level 2  

### 1 :

**hashcat -a 0 -m 1400 hash /usr/share/wordlists/rockyou.txt --force** 

### 2 :

**hashcat -a 0 -m 1000 hash /usr/share/wordlists/rockyou.txt --session --force** 

### 3 :

**hashcat -a 0 -m 1800 hash /usr/share/wordlists/rockyou.txt --force** 

### 4 :

**hashcat -a 0 -m 110 hash /usr/share/wordlists/rockyou.txt --force** 

-----------------------------------------