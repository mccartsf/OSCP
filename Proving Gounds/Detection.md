# About

This lab focuses on exploiting CVE-2024-32651, a Server-Side Template Injection (SSTI) vulnerability in Changedetection.io v0.45.1. By leveraging Jinja2's template engine, attackers can execute arbitrary commands, leading to Remote Code Execution (RCE) and a full server compromise.


## Skills Being Worked On

everage enumeration techniques to uncover potential vulnerabilities. The lab involves exploiting CVE-2024-32651 to demonstrate its impact on system security. This lab focuses on understanding and exploiting vulnerabilities to enhance security awareness.

## Used Tools 



# Information Gathering 

*Nmap Scans*

		nmap -sV -sC -v -p- 192.168.120.97

Results:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-06 18:11 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 18:11
Completed NSE at 18:11, 0.00s elapsed
Initiating NSE at 18:11
Completed NSE at 18:11, 0.00s elapsed
Initiating NSE at 18:11
Completed NSE at 18:11, 0.00s elapsed
Initiating Ping Scan at 18:11
Scanning 192.168.120.97 [4 ports]
Completed Ping Scan at 18:11, 0.08s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 18:11
Completed Parallel DNS resolution of 1 host. at 18:11, 0.51s elapsed
Initiating SYN Stealth Scan at 18:11
Scanning 192.168.120.97 [65535 ports]
Discovered open port 22/tcp on 192.168.120.97
Discovered open port 5000/tcp on 192.168.120.97
Completed SYN Stealth Scan at 18:12, 50.87s elapsed (65535 total ports)
Initiating Service scan at 18:12
Scanning 2 services on 192.168.120.97
Completed Service scan at 18:12, 6.41s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.120.97.
Initiating NSE at 18:12
Completed NSE at 18:12, 1.64s elapsed
Initiating NSE at 18:12
Completed NSE at 18:12, 0.23s elapsed
Initiating NSE at 18:12
Completed NSE at 18:12, 0.00s elapsed
Nmap scan report for 192.168.120.97
Host is up (0.090s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)
|   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)
|_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)
5000/tcp open  http    Python http.server 3.5 - 3.10
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET
|_http-title: Change Detection
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
Initiating NSE at 18:12
Completed NSE at 18:12, 0.00s elapsed
Initiating NSE at 18:12
Completed NSE at 18:12, 0.00s elapsed
Initiating NSE at 18:12
Completed NSE at 18:12, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.25 seconds
           Raw packets sent: 65639 (2.888MB) | Rcvd: 65636 (2.625MB)


Scan shows that ports ==22 & 500== are open

Port 5000 appears to be a python server running an io called  "CHANGEDETECTION.IO."

The currently running version of CHANGEDECTION.IO is v0.45.1.






# Foothold


# Privilege Escalation