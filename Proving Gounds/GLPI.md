

# Introduction

This lab exploits a Remote Code Execution (RCE) vulnerability in GLPI 10.0.2 (CVE-2022-35914) to achieve initial access. Attackers escalate privilege by leveraging a writable Jetty server's webapps directory to trigger a reverse shell through crafted XML configuration files. 
# Objective

- Enumerate open services and identify the GLPI 10.0.2 instance running on port 80.
- Exploit the GLPI RCE vulnerability using a crafted payload to obtain a reverse shell as www-data.
- Extract database credentials from the config_db.php file and use them to retrieve user passwords.
- SSH into the target as the betty user using extracted credentials.
- Exploit the writable Jetty server webapps folder to deploy a crafted XML configuration for a reverse shell, escalating to root access.
# Information Gathering

Nmap Scan results:
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-18 23:11 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 23:11
Completed NSE at 23:11, 0.00s elapsed
Initiating NSE at 23:11
Completed NSE at 23:11, 0.00s elapsed
Initiating NSE at 23:11
Completed NSE at 23:11, 0.00s elapsed
Initiating Ping Scan at 23:11
Scanning 192.168.165.242 [4 ports]
Completed Ping Scan at 23:11, 0.09s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 23:11
Completed Parallel DNS resolution of 1 host. at 23:11, 0.52s elapsed
Initiating SYN Stealth Scan at 23:11
Scanning 192.168.165.242 [65535 ports]
Discovered open port 22/tcp on 192.168.165.242
Discovered open port 80/tcp on 192.168.165.242
SYN Stealth Scan Timing: About 13.89% done; ETC: 23:14 (0:03:12 remaining)
SYN Stealth Scan Timing: About 39.49% done; ETC: 23:13 (0:01:33 remaining)
SYN Stealth Scan Timing: About 70.25% done; ETC: 23:13 (0:00:39 remaining)
Completed SYN Stealth Scan at 23:13, 118.39s elapsed (65535 total ports)
Initiating Service scan at 23:13
Scanning 2 services on 192.168.165.242
Completed Service scan at 23:13, 6.18s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.165.242.
Initiating NSE at 23:13
Completed NSE at 23:13, 5.07s elapsed
Initiating NSE at 23:13
Completed NSE at 23:13, 0.43s elapsed
Initiating NSE at 23:13
Completed NSE at 23:13, 0.00s elapsed
Nmap scan report for 192.168.165.242
Host is up (0.059s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 98:4e:5d:e1:e6:97:29:6f:d9:e0:d4:82:a8:f6:4f:3f (RSA)
|   256 57:23:57:1f:fd:77:06:be:25:66:61:14:6d:ae:5e:98 (ECDSA)
|_  256 c7:9b:aa:d5:a6:33:35:91:34:1e:ef:cf:61:a8:30:1c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Authentication - GLPI
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-favicon: Unknown favicon MD5: C01D32D71C01C8426D635C68C4648B09
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available ports ==22,80==

Gobuster results:

files                (Status: 301) [Size: 318] [--> http://192.168.165.242/files/]
js                   (Status: 301) [Size: 315] [--> http://192.168.165.242/js/]
lib                  (Status: 301) [Size: 316] [--> http://192.168.165.242/lib/]
css                  (Status: 301) [Size: 316] [--> http://192.168.165.242/css/]
public               (Status: 301) [Size: 319] [--> http://192.168.165.242/public/]
pics                 (Status: 301) [Size: 317] [--> http://192.168.165.242/pics/]
marketplace          (Status: 301) [Size: 324] [--> http://192.168.165.242/marketplace/]
config               (Status: 301) [Size: 319] [--> http://192.168.165.242/config/]
front                (Status: 301) [Size: 318] [--> http://192.168.165.242/front/]
src                  (Status: 301) [Size: 316] [--> http://192.168.165.242/src/]
vendor               (Status: 301) [Size: 319] [--> http://192.168.165.242/vendor/]
install              (Status: 301) [Size: 320] [--> http://192.168.165.242/install/]
ajax                 (Status: 301) [Size: 317] [--> http://192.168.165.242/ajax/]
sound                (Status: 301) [Size: 318] [--> http://192.168.165.242/sound/]
templates            (Status: 301) [Size: 322] [--> http://192.168.165.242/templates/]
bin                  (Status: 301) [Size: 316] [--> http://192.168.165.242/bin/]
inc                  (Status: 301) [Size: 316] [--> http://192.168.165.242/inc/]
plugins              (Status: 301) [Size: 320] [--> http://192.168.165.242/plugins/]

Using the gobuster results, we can begin to search through the used website index to properly find a potential vulnerability in the GLPI login page. Additionally, it is worth researching any available vulnerabilities on exploitDB and other sources.


# Service Enumeration 


# Privilege Escalation