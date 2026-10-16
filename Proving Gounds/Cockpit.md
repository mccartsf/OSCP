
# Introduction

In this lab, attackers manually exploit a SQL injection to obtain credentials and leverage write access to a user's filesystem for SSH access. Attackers then analyze an interesting ELF binary to discover a wildcard bug that facilitates privilege escalation to root access. 
# Objective

- Perform enumeration to identify a vulnerable login page.
- Exploit SQL Injection to bypass login authentication and extract credentials.
- Use the extracted credentials to gain access to the system via SSH.
- Analyze the AuthChecker SUID binary and identify the wildcard bug in its cp command.
# Information Gathering 

Nmap Scans:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-10-15 16:33 -0400  
NSE: Loaded 158 scripts for scanning.  
NSE: Script Pre-scanning.  
Initiating NSE at 16:33  
Completed NSE at 16:33, 0.00s elapsed  
Initiating NSE at 16:33  
Completed NSE at 16:33, 0.00s elapsed  
Initiating NSE at 16:33  
Completed NSE at 16:33, 0.00s elapsed  
Initiating Ping Scan at 16:33  
Scanning 192.168.139.10 [4 ports]  
Completed Ping Scan at 16:33, 0.11s elapsed (1 total hosts)  
Initiating Parallel DNS resolution of 1 host. at 16:33  
Completed Parallel DNS resolution of 1 host. at 16:33, 0.50s elapsed  
Initiating SYN Stealth Scan at 16:33  
Scanning 192.168.139.10 [65535 ports]  
Discovered open port 80/tcp on 192.168.139.10  
Discovered open port 22/tcp on 192.168.139.10  
SYN Stealth Scan Timing: About 23.17% done; ETC: 16:35 (0:01:43 remaining)  
Discovered open port 9090/tcp on 192.168.139.10  
Completed SYN Stealth Scan at 16:34, 78.90s elapsed (65535 total ports)  
Initiating Service scan at 16:34  
Scanning 3 services on 192.168.139.10  
Completed Service scan at 16:34, 6.16s elapsed (3 services on 1 host)  
NSE: Script scanning 192.168.139.10.  
Initiating NSE at 16:34  
Completed NSE at 16:35, 30.13s elapsed  
Initiating NSE at 16:35  
Completed NSE at 16:35, 0.36s elapsed  
Initiating NSE at 16:35  
Completed NSE at 16:35, 0.00s elapsed  
Nmap scan report for 192.168.139.10  
Host is up (0.054s latency).  
Not shown: 65532 closed tcp ports (reset)  
PORT     STATE SERVICE VERSION  
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)  
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)  
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)  
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))  
|_http-server-header: Apache/2.4.41 (Ubuntu)  
|_http-title: blaze  
| http-methods:    
|_  Supported Methods: GET POST OPTIONS HEAD  
9090/tcp open  http    Cockpit web service 198 - 220  
| http-methods:    
|_  Supported Methods: GET HEAD  
|_http-title: Did not follow redirect to https://192.168.139.10:9090/  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available ports: ==🟡22, 80, 9090==

Gobuster Results:
  
img                  (Status: 301) [Size: 314] [--> http://192.168.139.10/img/]  
js                   (Status: 301) [Size: 313] [--> http://192.168.139.10/js/]  
css                  (Status: 301) [Size: 314] [--> http://192.168.139.10/css/]  
Progress: 100000 / 100000 (100.00%)
# Service Enumeration

TBC (to be continued)
# Privilege Escalation

TBC (to be continued)
