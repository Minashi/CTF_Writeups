IPv4: 10.129.95.184

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.95.184 -p-
    
    Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-07 23:47 EDT
    Nmap scan report for 10.129.95.184
    Host is up (0.042s latency).
    Not shown: 65533 closed tcp ports (reset)
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey:
    |   2048 f65c9b38eca75c791c1f181c5246f70b (RSA)
    |   256 650cf7db42034607f21289fe11202c53 (ECDSA)
    |_  256 b865cd3f34d8026ae318233e77dd8740 (ED25519)
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    |_http-title: Welcome to Base
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 19.41 seconds
    
- sudo gobuster dir -u [http://10.129.95.184](http://10.129.95.184/) -w /usr/share/wordlists/dirb/common.txt
    
    /.htpasswd            (Status: 403) [Size: 278]
    /.hta                 (Status: 403) [Size: 278]
    /.htaccess            (Status: 403) [Size: 278]
    /assets               (Status: 301) [Size: 315] [--> http://10.129.95.184/assets/]
    /forms                (Status: 301) [Size: 314] [--> http://10.129.95.184/forms/]
    /index.html           (Status: 200) [Size: 39344]
    /login                (Status: 301) [Size: 314] [--> http://10.129.95.184/login/]
    /server-status        (Status: 403) [Size: 278]
    

- Users
    
    ****Walter White****
    
    ****Sarah Jhonson****
    
    ****William Anderson****
    
    ****Amanda Jepson****
    
    Saul Goodman
    
    Sara Wilsson
    
    Jena Karlis
    
    Matt Brandon
    

No SQL injections on login page

/login has a login.php.swp file

Downloaded and used strings

strcmp vulnerability

uploaded php revshell now I need to find an uploads directory to trigger it. First scan was not enough so using a bigger wordlist.

/dirbuster/directory-list-2.3-medium.txt was not enough so I tried /dirb/big.txt

- sudo gobuster dir -u http://10.129.95.184/ -w /usr/share/wordlists/dirb/big.txt -t 50
    
    /.htpasswd            (Status: 403) [Size: 278]
    /.htaccess            (Status: 403) [Size: 278]
    /_uploaded            (Status: 301) [Size: 318] [--> http://10.129.95.184/_uploaded/]
    /assets               (Status: 301) [Size: 315] [--> http://10.129.95.184/assets/]
    /forms                (Status: 301) [Size: 314] [--> http://10.129.95.184/forms/]
    /login                (Status: 301) [Size: 314] [--> http://10.129.95.184/login/]
    /server-status        (Status: 403) [Size: 278]
    

http://10.129.95.184/_uploaded/shell.php

shell

## Exploitation

## Privilege Escalation

Check config file where login.php compares out password input to for admin password

su to john for user flag

(root : root) /usr/bin/find

gtfo bin for find

rooted

## Post-exploitation

None
