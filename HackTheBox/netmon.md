**IPv4: 10.10.10.152**
## Enumeration/Scanning
sudo nmap -sV -sC -O -v $IP

ports
* 445
* 21
	* ftp
* 139
* 80
	* http web server
* 135

First I pursued FTP as port 21 allows anonymous login to the server.

I was able to navigate the directory and access the user flag, but the administrator directory is locked.
* Unable to upload file into web directory so no reverse shells so far

Accessing program files (x86) I can find the software for PRTG
* Googling where PRTG keeps its data stored
	* https://kb.paessler.com/en/topic/463-how-and-where-does-prtg-store-its-data
* PRTG stores data in C:\ProgramData\Paessler\PRTG Network Monitor

Documentation from above link does not mention PRTG Configuration.old.bak, only .old and .dat

inspecting this file I found the login credentials for admin
* prtgadmin
* PrTg@dmin2018
* I was unable to login with the current pass, so I tried 2019 and it worked.

The exploit PRTG is vuln to with its version
* https://nvd.nist.gov/vuln/detail/CVE-2018-9276

The exploit states that with admin priv on the web console, I can exploit an OS command injection vuln by sending malformed parameters in sensor or notification management scenarios.

This article https://codewatch.org/2018/06/25/prtg-18-2-39-command-injection-vulnerability/

states that remote code execution can work by triggering notifications within the web console. Following the article you go to setup account settings and notifications.

make a new notif and enable execute program option towards the bottom.

Using a one liner for adding a user and giving it admin, you can then save the notif and then execute it, creating a user with root access.
* abc.txt | net user htb abc123! /add ; net localgroup administrators htb /add

Now that a user has been added, we need to connect to the environment.

I learned about a new tool called psexec.py
* https://github.com/fortra/impacket/blob/master/examples/psexec.py
* This tool allows me to remotely connect to the server.
* psexec.py htb:'abc123!'@10.10.10.152

Found root flag









## Exploitation


## Privilege Escalation


## Post-exploitation
