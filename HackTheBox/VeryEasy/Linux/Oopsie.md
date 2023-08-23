# Oopsie

IPv4: 10.129.95.191

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.95.191 -p-
    
    Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-07 22:17 EDT
    Nmap scan report for 10.129.95.191
    Host is up (0.076s latency).
    Not shown: 65533 closed tcp ports (reset)
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey:
    |   2048 61e43fd41ee2b2f10d3ced36283667c7 (RSA)
    |   256 241da417d4e32a9c905c30588f60778d (ECDSA)
    |_  256 78030eb4a1afe5c2f98d29053e29c9f2 (ED25519)
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    |_http-title: Welcome
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 27.27 seconds
    
- sudo gobuster dir -u [http://10.129.95.191](http://10.129.95.191/) -w /usr/share/wordlists/dirb/common.txt
    
    /.htaccess            (Status: 403) [Size: 278]
    /.htpasswd            (Status: 403) [Size: 278]
    /.hta                 (Status: 403) [Size: 278]
    /css                  (Status: 301) [Size: 312] [--> http://10.129.95.191/css/]
    /fonts                (Status: 301) [Size: 314] [--> http://10.129.95.191/fonts/]
    /images               (Status: 301) [Size: 315] [--> http://10.129.95.191/images/]
    /index.php            (Status: 200) [Size: 10932]
    /js                   (Status: 301) [Size: 311] [--> http://10.129.95.191/js/]
    /server-status        (Status: 403) [Size: 278]
    /themes               (Status: 301) [Size: 315] [--> http://10.129.95.191/themes/]
    /uploads              (Status: 301) [Size: 316] [--> http://10.129.95.191/uploads/]
    
- sudo gobuster dir -u [http://10.129.95.191](http://10.129.95.191/) -w /usr/share/wordlists/dirb/common.txt -x .js,.php
    
    /.php                 (Status: 403) [Size: 278]
    /.hta                 (Status: 403) [Size: 278]
    /.hta.php             (Status: 403) [Size: 278]
    /.htaccess            (Status: 403) [Size: 278]
    /.hta.js              (Status: 403) [Size: 278]
    /.htaccess.js         (Status: 403) [Size: 278]
    /.htpasswd            (Status: 403) [Size: 278]
    /.htaccess.php        (Status: 403) [Size: 278]
    /.htpasswd.php        (Status: 403) [Size: 278]
    /.htpasswd.js         (Status: 403) [Size: 278]
    /css                  (Status: 301) [Size: 312] [--> http://10.129.95.191/css/]
    /fonts                (Status: 301) [Size: 314] [--> http://10.129.95.191/fonts/]
    /images               (Status: 301) [Size: 315] [--> http://10.129.95.191/images/]
    /index.php            (Status: 200) [Size: 10932]
    /index.php            (Status: 200) [Size: 10932]
    /js                   (Status: 301) [Size: 311] [--> http://10.129.95.191/js/]
    /server-status        (Status: 403) [Size: 278]
    /themes               (Status: 301) [Size: 315] [--> http://10.129.95.191/themes/]
    /uploads              (Status: 301) [Size: 316] [--> http://10.129.95.191/uploads/]
    

So far I have not found anything with directory enum…

Trying Nikto as well as checking service version exploits

- nikto -url 10.129.95.191
    - Server: Apache/2.4.29 (Ubuntu)
    - /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
    - /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
    ^[[A+ No CGI Directories found (use '-C all' to force check all possible dirs)
    - Apache/2.4.29 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
    - /images: IP address found in the 'location' header. The IP is "127.0.1.1". See: https://portswigger.net/kb/issues/00600300_private-ip-addresses-disclosed
    - /images: The web server may reveal its internal or real IP in the Location header via a request to with HTTP/1.0. The value is "127.0.1.1". See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2000-0649
    - /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
    - : CGI Directory found. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-1607
    - /cdn-cgi/login/: CGI Directory found. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-1607
    - /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
    ^C

cdn-cgi/login gives me a login page

No sql vulnerabilities. There is guest login

## Exploitation

manipulating the get query I can view the access id for admin

http://10.129.95.191/cdn-cgi/login/admin.php?content=accounts&id=1

guest [guest@megacorp.com](mailto:guest@megacorp.com) 2233

admin [admin@megacorp.com](mailto:admin@megacorp.com) 34322

checking burpsuite I notice I may be able to manipulate some headers

I may be able to pretend to be admin to access uploads

I was able to!

Now to upload a reverse shell. I know the server is running php so I’ll use that

http://10.129.95.191/uploads/shell.php

shell

## Privilege Escalation

check SUID and found pkexec and the kernal version leads to pwnkit

uploaded pwnkit to /tmp and ran it to root the server

## Post-exploitation

None
