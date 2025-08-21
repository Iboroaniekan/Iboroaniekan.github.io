# VulnHub DC-1 Walkthrough: From Drupalgeddon to Root

**Date:** February 10, 2022
**Platform:** VulnHub
**Difficulty:** Beginner

## Introduction
DC-1 is a deliberately vulnerable machine designed for beginners to practice penetration testing skills. The goal is to get "root" access on the machine.

## Step 1: Reconnaissance
I started by finding the machine's IP address on my network using the `netdiscover` command.

Then, I scanned the machine with `nmap` to see what doors were open:

```bash
nmap -sV -sC 192.168.1.123
```
*   **-sV**: Checks what software versions are running.
*   **-sC**: Runs basic scripts to get more info.

The scan showed port 80 was open, running a Drupal website.

## Step 2: Exploitation - Getting a Foothold
I knew Drupal 7 was old and might have known vulnerabilities. I searched for "Drupal 7 exploit" and found "Drupalgeddon 2".

I used an exploit script I found to execute code on the target.

```bash
python3 drupalgeddon.py http://192.168.1.123
```
It worked! I got a low-level shell on the machine.

## Step 3: Privilege Escalation - Becoming Root
I was logged in as a user called `www-data`. I needed to become `root`. I looked for files with special permissions using the `find` command:

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
