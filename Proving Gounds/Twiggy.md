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


# Service Enumeration


# Privilege Escalation