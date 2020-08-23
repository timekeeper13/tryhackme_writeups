# Tryhackme Room Link
**https://tryhackme.com/room/rpwebscanning**

----------------

## [Task 1] Pull the lever, Kronk! 

Deploy the machine!

----------------

## [Task 2] ...I'm supposed to scan with that? 

nikto -h 

### 1  First and foremost, what switch do we use to set the target host?
**-h**

### 2  disalbe ssl
**-nossl**

### 3 force secure transport
**-ssl**

### 4 
**-p**

### 5
**--dbcheck**

### 6
**-mutate 3**

### 7
**-id admin:PrettyAwesomePassword1234**

### 8

lets scan our machine  
**nikto -h <machine ip>**  
its running on Apache/2.4.7  

### 9
This directory allows for settings to potentially be changed remotely, allowing for us to do silly things just as just removing the metaphorical lock from the door.  
**config**

### 10
to limit the scan  
**-until**

### 11
To list all the plugins available   
**-list-plugins**

### 12
to find the outdated softwares on the server   
**-plugins outdated**

### 13
Finally, what if we'd like to use our plugins to run a series of standard tests against the target host?  
**-plugins test**

--------------------

## [Task 3] Zip ZAP! 

### 1  
lets launch the zap  
**owasp-zap**

### 2
Launch ZAP, what option to we set in order to specify what we are attacking?  
**url to attack**

### 3
launch the attack

### 4
**robots.txt**

### 5
One entry is included in the disallow section of this file, what is it?  
**/**

### 6
**/dvwa/images**

### 7
look in alerts  
**HttpOnly**

### 8
you can find it in alerts  
**Web Browser XSS Protection is not enabled**

### 9
The ZAP proxy spider represents the component responsible for 'crawling' the site. What site is found to be out of scope?  
**http://www.dvwa.co.uk/**

### 10
ZAP will use primarily two methods in order to scan a website, which of these two HTTP methods requests content?  
**get**

### 11
Which option attempts to submit content to the website?  
**post**

