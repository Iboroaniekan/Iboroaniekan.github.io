# VulnHub DC-1 Walkthrough: From Drupalgeddon to Root

**Date:** August 25, 2025
**Platform:** VulnHub
**Difficulty:** Beginner

## Introduction
DC-1 is a deliberately vulnerable machine designed for beginners to practice penetration testing skills. The goal is to get "root" access on the machine.

## Step 1: Reconnaissance
I started by finding the machine's IP address on my network using the `netdiscover -i eth0` command.

![Results of the netdiscover scan showing IP](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/netdiscover.png?raw=true)

Then, I scanned the machine with `nmap` to see what services are running :

![Results of the Nmap scan showing services](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/nmap%20service.png?raw=true)

```bash
nmap -sS -sV -O 10.0.2.7
```
*   **-O**: Checks for what operating system
*   **-sV**: Checks for the service version

  Then i also used nmap to run a script scan :

 ![Results of the Nmap scan showing services](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/nmap%20scriptscan.png?raw=true)

 Also the script scan showed that a robots.txt file was found,The scan showed port 80 was open, running a Drupal website.

 INFORMATION DISCLOSURE:
 server was revealing information about its version number :
 
 ![Results of the Apache info](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/apache.png?raw=true)

 I also used wappalyzer to see what technologies was used in building the website and i discovered drupal 7 was running revealing also the version number of the content management system (CMS).
 
 ![Results of the wappalyzer](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/wappalyzer.png?raw=true)
  


## Step 2: Discovering vulnerabilities - Using Nikto

 ## (root㉿kali) - [/root@Kali] 
 
 ## nikto --host http://10.0.2.7
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.0.2.7
+ Target Hostname:    10.0.2.7
+ Target Port:        80
+ Start Time:         2025-11-15 12:28:05 (GMT1)
---------------------------------------------------------------------------
+ Server: Apache/2.2.22 (Debian)
+ /: Retrieved x-powered-by header: PHP/5.4.45-0+deb7u14.
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: Drupal 7 was identified via the x-generator header. See: https://www.drupal.org/project/remove_http_headers
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ /robots.txt: Server may leak inodes via ETags, header found with file /robots.txt, inode: 152289, size: 1561, mtime: Wed Nov 20 21:45:59 2013. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ /robots.txt: Entry '/INSTALL.sqlite.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/install.php' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/user/login/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/LICENSE.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/?q=user/register/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/MAINTAINERS.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/?q=user/login/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/?q=filter/tips/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/?q=user/password/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/filter/tips/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/INSTALL.mysql.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/xmlrpc.php' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/UPGRADE.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/user/password/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/user/register/' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: Entry '/INSTALL.pgsql.txt' is returned a non-forbidden or redirect HTTP code (200). See: https://portswigger.net/kb/issues/00600600_robots-txt-file
+ /robots.txt: contains 36 entries which should be manually viewed. See: https://developer.mozilla.org/en-US/docs/Glossary/Robots.txt
+ Apache/2.2.22 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ /misc/favicon.ico: identifies this app/server as: Drupal 7.x. See: https://en.wikipedia.org/wiki/Favicon
+ /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
+ /: DEBUG HTTP verb may show server debugging information. See: https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-enable-debugging-for-aspnet-applications?view=vs-2017
+ /web.config: ASP config file is accessible.

After using Nikto to scan for vulnerable services i decided to try manually using google to look for vulnerability on drupal7.I looked at exploit database on google and found a known vulnerabilty.

![Results of the exploitdb ](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/exploitdb.png?raw=true)

I also used built in searchsploit on my kali machine to search for vulnerabilty and i discovered there is a vulnerabilty that could make me add a user as an admin on the website.

![Results of searchsploit](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/searchsploit.png?raw=true)


I also used Metasploit framework to search for any vulnerabilty that i can use to gain shell access on the machine and i found one 

![Results of msf](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/msf.png?raw=true)


 Drupal 7.x has a known vulnerability found in exploit database and on metasploit .

 1. Vulnerable to Remote Code Execution ( Drupalgeddon2   )

2. Apache/2.2.22 appears to be outdated (current is at least Apache/2.4.54). 

   ## Step 3 :  Exploitation

After looking for vulnerabilty on different database i decided to use an exploit found in metasploit to gain access to a meterpreter shell

![Results of msfpayload](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/msf%20payload.png?raw=true)

Now i exploited the machine and got a meterpreter shell 

![Results of msfpayload](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/shell.png?raw=true)

now after getting access to the machine its a low level access and i need to find the first flag 

![Results of firstflag](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/flag1.png?raw=true)

After finding the first flag it says Every good CMS needs a config file - and so do you.

Now looking for the config file will be found in the sites folder and i cant get access to it so i need to perform privilege escalation


## Step 3: Privilege Escalation
I was logged in as a user called `www-data`. I needed to get a proper shell so i used this script to spawn the shell 

Python -c 'import pty; pty.spawn("/bin/bash")'

![Results of spawnshell](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/spawn%20shell.png?raw=true)

After i spawn the shell i got access to the sites folder and changed directory to it 

![Results of sites](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/sites.png?raw=true)

Now after viewing the files inside the sites folder i saw a settings.php file and i opened it and found credentials related to the database and i also found flag2 in it 

![Results of database](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/database.png?raw=true)

Now after seeing the information related to the database i decided to connect to the mysql server since i used netstat and discovered mysql is open on the machine 

![Results of mysql](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/data%20info.png?raw=true)

Now i switched to view the tables in the database to look for interesting items:

![Results of tables](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/tables.png?raw=true)

after viewing the table i selected the users table using select * FROM users; and i got users credentials,emails.

![Results of userinfo](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/user%20info.png?raw=true)

Now after seeing the users information i tried to crack the password but wasnt successful so i decided to use the exploit found in searchsploit to add a user as an admin.

![Results of adminuser](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/adminuser.png?raw=true)

Now i have successfully added a user as an admin with the details below :

![Results of adminaccess](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/adminaccess.png?raw=true)

After successfully creating a user i decided to login to the admin panel :

![Results of login](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/flag2.png?raw=true)
I logged in and found flag 3 there.

Here is also flag4 found 
![Results of flag4](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/flag4.png?raw=true)


Now i need to escalate to become root user on this machine and i first of all looked for binaries that i could use to exploit the machine

![Results of find](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/find.png?raw=true)

after confirming that Find is there I looked for files with special permissions using the `find` command:

![Results of perm](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/perm.png?raw=true)

```bash
find / -perm -u=s -type f 2>/dev/null
```
After confirming it i went to gtfobins to look for command i could use to root the machine and i found one ;

![Results of root](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/root.png?raw=true)


after exploiting it i ran the whoami command to prove if i am now root on this machine and i was root so i need to look for the final flag in the root directory

![Results of finalflag](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/finalflag.png?raw=true)


## Conclusion
This was a fun machine! I learned how to:
*   Use `nmap` for scanning.
*   Research and use a public exploit.
*   Escalate privileges on a Linux system.

**Remember: This was done on a practice machine from VulnHub. Always hack responsibly and only on systems you own or have permission to test!**
