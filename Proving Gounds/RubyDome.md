

# Introduction

This lab will demonstrate a Remote Code Execution (RCE) vulnerability in the PDFKit gem (CVE-2022-25765) used in a RubyDome HTML-to-PDF conversion service. Privilege escalation is leverage by a sudo misconfiguration allowing the user "andrew" to execute a Ruby script (app.rb) with root privileges. By injecting a malicious payload into the script, the attacker is able gain a root shell. 
# Objective

- Enumerate open ports to identify the RubyDome service running on port 3000.
- Exploit the PDFKit RCE vulnerability (CVE-2022-25765) to achieve a reverse shell 
- Enumerate sudo privileges for the andrew user to discover executable Ruby scripts.
- Modify the app.b script to inject a malicious payload for command execution.
- Execute the script using sudo and escalate privileges to root. 


# Information Gathering 

Nmap Scan Results:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-16 21:54 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 21:54
Completed NSE at 21:54, 0.00s elapsed
Initiating NSE at 21:54
Completed NSE at 21:54, 0.00s elapsed
Initiating NSE at 21:54
Completed NSE at 21:54, 0.00s elapsed
Initiating Ping Scan at 21:54
Scanning 192.168.220.22 [4 ports]
Completed Ping Scan at 21:54, 0.16s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:54
Completed Parallel DNS resolution of 1 host. at 21:54, 0.52s elapsed
Initiating SYN Stealth Scan at 21:54
Scanning 192.168.220.22 [65535 ports]
Discovered open port 22/tcp on 192.168.220.22
SYN Stealth Scan Timing: About 45.74% done; ETC: 21:55 (0:00:37 remaining)
Discovered open port 3000/tcp on 192.168.220.22
Completed SYN Stealth Scan at 21:55, 60.82s elapsed (65535 total ports)
Initiating Service scan at 21:55
Scanning 2 services on 192.168.220.22
Completed Service scan at 21:55, 6.44s elapsed (2 services on 1 host)
NSE: Script scanning 192.168.220.22.
Initiating NSE at 21:55
Completed NSE at 21:55, 2.15s elapsed
Initiating NSE at 21:55
Completed NSE at 21:55, 0.41s elapsed
Initiating NSE at 21:55
Completed NSE at 21:55, 0.00s elapsed
Nmap scan report for 192.168.220.22
Host is up (0.084s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b9:bc:8f:01:3f:85:5d:f9:5c:d9:fb:b6:15:a0:1e:74 (ECDSA)
|_  256 53:d9:7f:3d:22:8a:fd:57:98:fe:6b:1a:4c:ac:79:67 (ED25519)
3000/tcp open  http    WEBrick httpd 1.7.0 (Ruby 3.0.2 (2021-07-07))
|_http-server-header: WEBrick/1.7.0 (Ruby/3.0.2/2021-07-07)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: RubyDome HTML to PDF
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available open port ==22, 3000==


On port 3000, the RubyDome HTML to PDF converter is running on the target IP (http://192.168.220.22:3000/).

The webpage host on the target IP is only taking the URL of webpages containing HTML code. 

To get a response from the website I have spun up a Pytnon server on port 8000 to envoke a response from the server:

		python3 -m http.server 8000

By placing in the webpage of the localhost:8000 page I have gotten an error response from the currently runnign service which include:

"# **PDFKit::ImproperWkhtmltopdfExitStatus** at **/pdf**..."

With this we get some information regardi the currently used packages. For example, the currently used packages are PDFKit and wkhtmltopdf v0.12.6.


# Service Enumeration 



# Privilege Escalation

















