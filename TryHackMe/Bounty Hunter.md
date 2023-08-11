**IPv4: 10.10.230.52**

## Enumeration/Scanning

sudo nmap -sC -sV -O -v 10.10.230.52 -o nmap/initial

```
ports
* 80
	* Apache httpd 2.4.18 ((Ubuntu))
* 22
* 21
	* ftp
	* vsftpd 3.0.3
* 990
	* ftps
* 44442/tcp closed coldfusion-auth
* 44443/tcp closed coldfusion-auth
50000/tcp closed ibm-db2
49400/tcp closed compaqdiag
```

Gobuster

nothing useful

I connected to the server using ftp
username: anonymous

no password

there were 2 files, one with a username and one with password

Username: lin

used hydra to bruteforce ssh
pass: RedDr4gonSynd1cat3

got user.txt

## Exploitation

## Privilege Escalation

checked sudo -l

can use tar with sudo

checked GTFO bins for sudo one liners

got root

## Post-exploitation

None
