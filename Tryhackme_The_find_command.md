# Tryhackme Room Address

***https://tryhackme.com/room/thefindcommand***

A learn-by-doing approach to the find command

## [Task 1] Start finding 

## [Task 2] Be more specific 

### 1 :  Find all files whose name ends with ".xml"
```
 find / -type f -name *.xml
```
### 2 : Find all files in the /home directory (recursive) whose name is "user.txt" (case insensitive)

**find /home -type f -name user.txt**

### 3 : Find all directories whose name contains the word "exploits"
```
find / -type d -name *exploits*
```

## [Task 3] Know exactly what you're looking for 

### 1 :  Find all files owned by the user "kittycat"

**find / -type f -user kittycat**

### 2 :  Find all files that are exactly 150 bytes in size

**find / -type f -size 150c**

### 3 : Find all files in the /home directory (recursive) with size less than 2 KiB’s and extension ".txt"
```
find /home -type f -size -2k -name *.txt
```

### 4 :  Find all files that are exactly readable and writeable by the owner, and readable by everyone else (use octal format)

**find / -type f -perm 644**

### 5 :  Find all files that are only readable by anyone (use octal format)

**find / -type f -perm /444**

### 6 :  Find all files with write permission for the group "others", regardless of any other permissions, with extension ".sh" (use symbolic format)
```
find / -type f -perm -o=w -name "*.sh"
```

### 7 :  Find all files in the /usr/bin directory (recursive) that are owned by root and have at least the SUID permission (use symbolic format)
```
find /usr/bin -type f -perm -u=s -user root
```
### 8 :  Find all files that were not accessed in the last 10 days with extension ".png"
```
find / -type f -atime +10 -name *.png
```
### 9 :  Find all files in the /usr/bin directory (recursive) that have been modified within the last 2 hours

```
find /usr/bin -type f -mmin -120 
```

## [Task 4] Have you found it? 

To conclude this tutorial, there are two more things that you should know of. The first is that you can use the redirection operator > with the find command. You can save the results of the search to a file, and more importantly, you can suppress the output of any possible errors to make the output more readable. This is done by appending 2> /dev/null to your command. This way, you won’t see any results you’re not allowed to access.


The second thing is the -exec flag. You can use it in your find command to execute a new command, following the -exec flag, like so: -exec whoami \;. The possibilities enabled by this option are beyond the scope of this tutorial, but most notably it can be used for privilege escalation.