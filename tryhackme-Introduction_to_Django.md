# Tryhackme Room Address

https://tryhackme.com/room/django

-----------

## Task 1:

Learning Python can be extremely useful for penetration testers and a simple understanding of its frameworks can be a key to success. In this lesson, we are going to learn about one of the best ones ever made: Django. 

-----------

## Task 2:

### 1
python3 manage.py startapp Forms

### 2
> **python3 manage.py runserver 0.0.0.0:8000**

-----------

## Task 3:

Create an app using a command from Unit 2 and call it whatever you like. 
Head over to settings.py and include your app name in *INSTALLED_APPS*:

-----------

## Task 4:

### 1
flag from the github page
THM{g1t_djang0_hUb}

-----------

## Task 5:

Now it's time for a small CTF!

some credentials given

> **Username: django-admin  
> Password: roottoor1212**

## Enumeration

> **nmap -sC -sV -oN nmap/django_scan <machine_IP>**

We can see SSH running on port 22
and http-alt running on 8000

scan runs we can visit http://<Machine IP>:8000    
we see a Django page with lots of messages about DISALLOWED HOSTS  
So to access it we need to add the machine ip to the allowed hosts  

### 1: Admin panel flag

with the given credentials we cn ssh into the machine  
and change the line in the settings file   

> **ALLOWED_HOSTS = ['0.0.0.0', '10.10.147.62']**  

include our machine ip to accesshttps://tryhackme.com/room/django it in browser  
we can now visit the admin login page   http://<machine IP>:8000/admin  
we have many options to login  

--> we can change the password of current admin

with  **python3 manage.py changepassword <admin_name >**
and then with that new password we can login to admin portal  
but this is not the recommended way


-->we can create a super user

Navigate to  *** /home/django-admin/messagebox/***  
Execute the command to create the superuser   
> **python3 manage.py createsuperuser**   

Back on the admin page, you can use those credentials to login.  
here is your first flag  

!
### 2 : user flag?

you have many options here 

-->   
Once you are logged in as django-admin
run ls -la /home you can see that you have access to another user's home folder  
 > **StrangeFox**     
 Let's get his flag!


> **ls -la /home  
> cd /home/StrangeFox  
> ls    
> cat user.txt  
> THM{xxx}**  

--> OR  
you can find a password hash link on the page

StrangeFox         Password hash:   > ***https://pastebin.com/nmKt4BSf***  

visit the link and get the link   
crack it with crackctation   > >**https://crackstation.net/**>  

with the credentials   

you can open a new ssh shell and get the flag   
or change the current user with su command  


### 3 : Hidden Flag

there are many ways of getting flag

-->  

get to the home page and   

> **grep -r 'THM'**
there is your flag  

-->  OR  

get to the messagebox and you can find home.html
cat it out and here is your final flag
