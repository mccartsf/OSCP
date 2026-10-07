
# Introduction


# Objective

Attempt 3
# Information Gathering 

Nmap Scans:
Starting Nmap 7.99 ( https://nmap.org ) at 2026-10-07 14:38 -0400  
NSE: Loaded 158 scripts for scanning.  
NSE: Script Pre-scanning.  
Initiating NSE at 14:38  
Completed NSE at 14:38, 0.00s elapsed  
Initiating NSE at 14:38  
Completed NSE at 14:38, 0.00s elapsed  
Initiating NSE at 14:38  
Completed NSE at 14:38, 0.00s elapsed  
Initiating Ping Scan at 14:38  
Scanning 192.168.139.240 [4 ports]  
Completed Ping Scan at 14:38, 0.15s elapsed (1 total hosts)  
Initiating Parallel DNS resolution of 1 host. at 14:38  
Completed Parallel DNS resolution of 1 host. at 14:38, 3.79s elapsed  
Initiating SYN Stealth Scan at 14:38  
Scanning 192.168.139.240 [65535 ports]  
Discovered open port 80/tcp on 192.168.139.240  
Discovered open port 22/tcp on 192.168.139.240  
Discovered open port 139/tcp on 192.168.139.240  
Discovered open port 445/tcp on 192.168.139.240  
SYN Stealth Scan Timing: About 11.17% done; ETC: 14:42 (0:04:06 remaining)  
SYN Stealth Scan Timing: About 22.50% done; ETC: 14:42 (0:03:30 remaining)  
SYN Stealth Scan Timing: About 37.65% done; ETC: 14:42 (0:02:31 remaining)  
SYN Stealth Scan Timing: About 53.60% done; ETC: 14:41 (0:01:45 remaining)  
SYN Stealth Scan Timing: About 74.29% done; ETC: 14:41 (0:00:52 remaining)  
Discovered open port 8021/tcp on 192.168.139.240  
Discovered open port 3000/tcp on 192.168.139.240  
Completed SYN Stealth Scan at 14:41, 183.83s elapsed (65535 total ports)  
Initiating Service scan at 14:41  
Scanning 6 services on 192.168.139.240  
Completed Service scan at 14:41, 11.45s elapsed (6 services on 1 host)  
NSE: Script scanning 192.168.139.240.  
Initiating NSE at 14:41  
Completed NSE at 14:42, 46.22s elapsed  
Initiating NSE at 14:42  
Completed NSE at 14:42, 0.21s elapsed  
Initiating NSE at 14:42  
Completed NSE at 14:42, 0.02s elapsed  
Nmap scan report for 192.168.139.240  
Host is up (0.051s latency).  
Not shown: 65529 filtered tcp ports (no-response)  
PORT     STATE SERVICE          VERSION  
22/tcp   open  ssh              OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
| ssh-hostkey:    
|   2048 74:ba:20:23:89:92:62:02:9f:e7:3d:3b:83:d4:d9:6c (RSA)  
|   256 54:8f:79:55:5a:b0:3a:69:5a:d5:72:39:64:fd:07:4e (ECDSA)  
|_  256 7f:5d:10:27:62:ba:75:e9:bc:c8:4f:e2:72:87:d4:e2 (ED25519)  
80/tcp   open  http             Apache httpd 2.4.38  
|_http-server-header: Apache/2.4.38 (Debian)  
|_http-title: 403 Forbidden  
| http-methods:    
|_  Supported Methods: HEAD GET POST OPTIONS  
139/tcp  open  netbios-ssn      Samba smbd 3.X - 4.X (workgroup: WORKGROUP)  
445/tcp  open  netbios-ssn      Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)  
3000/tcp open  http             Thin httpd  
|_http-title: Cassandra Web  
|_http-favicon: Unknown favicon MD5: 68089FD7828CD453456756FE6E7C4FD8  
|_http-server-header: thin  
| http-methods:    
|_  Supported Methods: GET HEAD  
8021/tcp open  freeswitch-event FreeSWITCH mod_event_socket  
Service Info: Hosts: 127.0.0.1, CLUE; OS: Linux; CPE: cpe:/o:linux:linux_kernel  
  
Host script results:  
| smb2-time:    
|   date: 2026-10-07T18:41:03  
|_  start_date: N/A  
| smb-os-discovery:    
|   OS: Windows 6.1 (Samba 4.9.5-Debian)  
|   Computer name: clue  
|   NetBIOS computer name: CLUE\x00  
|   Domain name: pg  
|   FQDN: clue.pg  
|_  System time: 2026-10-07T14:41:07-04:00  
|_clock-skew: mean: 1h19m40s, deviation: 2h18m37s, median: -21s  
| smb-security-mode:    
|   account_used: guest  
|   authentication_level: user  
|   challenge_response: supported  
|_  message_signing: disabled (dangerous, but default)  
| smb2-security-mode:    
|   3.1.1:    
|_    Message signing enabled but not required


Available ports ==🟡22, 80, 139, 445, 3000, 8021==

Found Credentials
cassie
SecondBiteTheApple330
# Service Enumeration


# Privilege Escalation