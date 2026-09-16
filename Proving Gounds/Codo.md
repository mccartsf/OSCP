# About

This lab demonstrates exploiting a Remote Code Execution (RCE) vulnerability in Codoforum (CVE-2022-31854) by uploading a malicious PHP shell through the "Upload Logo" feature in the admin panel. The vulnerability allows an attacker to execute arbitrary commands as the www-data user. 
## Skills Being Worked On

In this lab, we perform web enumeration to discover potential vulnerabilities on malicious file upload techniques. After identifying the vulnerabilities, we will exploit a file upload functionality to gain access to the system. 
# Information Gathering 

Nmap Results:

tarting Nmap 7.99 ( https://nmap.org ) at 2026-09-16 18:23 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Initiating NSE at 18:23
Completed NSE at 18:23, 0.00s elapsed
Initiating Ping Scan at 18:23
Scanning 192.168.220.23 [4 ports]
Completed Ping Scan at 18:23, 0.06s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 18:23
Completed Parallel DNS resolution of 1 host. at 18:23, 0.50s elapsed
Initiating SYN Stealth Scan at 18:23
Scanning 192.168.220.23 [65535 ports]
Discovered open port 22/tcp on 192.168.220.23
Discovered open port 80/tcp on 192.168.220.23
SYN Stealth Scan Timing: About 15.44% done; ETC: 18:26 (0:02:50 remaining)
SYN Stealth Scan Timing: About 43.22% done; ETC: 18:25 (0:01:20 remaining)
SYN Stealth Scan Timing: About 67.30% done; ETC: 18:25 (0:00:44 remaining)
Completed SYN Stealth Scan at 18:25, 123.61s elapsed (65535 total ports)
Initiating Service scan at 18:25
Scanning 2 services on 192.168.220.23
Completed Service scan at 18:25, 6.32s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.220.23.
Initiating NSE at 18:25
Completed NSE at 18:25, 5.12s elapsed
Initiating NSE at 18:25
Completed NSE at 18:25, 0.40s elapsed
Initiating NSE at 18:25
Completed NSE at 18:25, 0.00s elapsed
Nmap scan report for 192.168.220.23
Host is up (0.055s latency).
Not shown: 65533 filtered tcp ports (no-response)
Bug in http-generator: no string output.
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)
|   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)
|_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: All topics | CODOLOGIC
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Open ports available ==22,80==


When visiting the target IP we are met on a CODOLOGIC homepage with a login screen. Using a social engineering attempt we are able to gain access with a common username/password combo:

username: admin
password: admin


Since we are logged into the "admin" account, we are able to access the subdomain "admin." This page allows us to enter into the backend of the HTML site with more detailed information. 

For example, we now know that the currently available version of CODOFORUM running is v5.1.105.

Using this information we can check for vulnerabilities previosuly discovered for this software.

Successfully, CODOFORUM is susceptible to remote code execution (RCE) in accordance with CVE 2022-31854. Using exploitDB, we download a known exploit and give it a test run:

exploitDB script -> https://www.exploit-db.com/exploits/50978



After running the exploit we do not received any feedback from the server. Since there was no response, we perform more research on available vulnerabilities. 

With luck, we find a CVE posted that mentions that the "logo" upload on the global settings page is directly vulnerable to malicious php code as there is no server side checks.

To perform this vulnerability we create a file, "example.php" with a reverse shell payload created on RevShells (https://www.revshells.com/), that will allow us entrance into the machine. 

Ex.

![](Pasted%20image%2020260916183539.png)

# Foothold


# Privilege Escalation




