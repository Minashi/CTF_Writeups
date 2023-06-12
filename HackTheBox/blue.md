**IPv4: 10.10.10.40**
## Enumeration/Scanning
sudo nmap -A -Pn -sS $IP

ports
* 135
	* msrpc
* 139
	* netbios-ssn
* 445
	* microsoft-ds
	* smb
* 40000+
	* msrpc ports

To find vuln
* sudo nmap --script vuln 10.10.10.40

MS17-010
* Eternal blue
* smb exploit

using msfconsole and entering options, I rooted the server and retrieved the flags.

## Exploitation


## Privilege Escalation


## Post-exploitation

