
# About

This lab exploits a Remote Code Execution (RCE) vulnerability in FuguHub 8.1 (CVE-2023-24078) through the file upload feature. By crafting a malicious .lsp file, attackers execute arbitrary system commands and gain a reverse shell. The highlights of this lab are file upload abuse, Lua scripting for exploitation, and achieving initial access on the target system.

## Skills Being Worked On

This lab focuses on enumeration techniques, specifically web enumeration, to identify potential vulnerabilities. The lab also covers exploiting a malicious file upload vulnerability to gain initial access. 

# Information Gathering 

Nmap Scan Results:

Initiating Service scan at 15:52
Scanning 4 services on 192.168.220.25
Completed Service scan at 15:54, 153.54s elapsed (4 services on 1 host)
NSE: Script scanning 192.168.220.25.
Initiating NSE at 15:54
Completed NSE at 15:54, 2.79s elapsed
Initiating NSE at 15:54
Completed NSE at 15:54, 0.33s elapsed
Initiating NSE at 15:54
Completed NSE at 15:54, 0.00s elapsed
Nmap scan report for 192.168.220.25
Host is up (0.048s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)
|   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)
|_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)
80/tcp   open  http       nginx 1.18.0
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: nginx/1.18.0
|_http-title: 403 Forbidden
8082/tcp open  http       Barracuda Embedded Web Server
|_http-favicon: Unknown favicon MD5: FDF624762222B41E2767954032B6F1FF
|_http-server-header: BarracudaServer.com (Posix)
|_http-title: Home
| http-methods: 
|   Supported Methods: OPTIONS GET HEAD PROPFIND PATCH POST PUT COPY DELETE MOVE MKCOL PROPPATCH LOCK UNLOCK
|_  Potentially risky methods: PROPFIND PATCH PUT COPY DELETE MOVE MKCOL PROPPATCH LOCK UNLOCK
| http-webdav-scan: 
|   Server Date: Sat, 12 Sep 2026 19:54:27 GMT
|   WebDAV type: Unknown
|   Allowed Methods: OPTIONS, GET, HEAD, PROPFIND, PATCH, POST, PUT, COPY, DELETE, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK
|_  Server Type: BarracudaServer.com (Posix)
9999/tcp open  ssl/abyss?
| ssl-cert: Subject: commonName=FuguHub/stateOrProvinceName=California/countryName=US
| Subject Alternative Name: DNS:FuguHub, DNS:FuguHub.local, DNS:localhost
| Issuer: commonName=Real Time Logic Root CA/organizationName=Real Time Logic LLC/countryName=US
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2019-07-16T19:15:09
| Not valid after:  2074-04-18T19:15:09
| MD5:     6320 2067 19be be32 18ce 3a61 e872 cc3f
| SHA-1:   503c a62d 8a8c f8c1 6555 ec50 77d1 73cc 0865 ec62
|_SHA-256: f9fc d0bb 8a2f 49a1 52bd 3615 904e 31fd 2d9e 2223 c5a3 a909 f6ee 2032 dca2 ea15
|_ssl-date: 2026-09-12T19:54:29+00:00; -25s from scanner time.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: -25s


Available ports 22, 80, 8082, 9999

After investigation, the target IP address is running FuguHub version 8.4 and directly allowed us to set an administration password on port 8082. 

FuguHub version 8.4 is directly vulnerable to remote code execution (RCE). An exploit that will help escalate our privileges can be found on Sploitus -> https://sploitus.com/exploit?id=KITPLOIT:TOOLS-GITHUB-SANJINDEDIC-FUGUHUB-8.4-AUTHENTICATED-RCE-CVE-2024-27697



# Foothold

Using the available Lua script on the exploit site, we navigate to the "CMS Admin" and create a new page under the "Page Manager." The title of the page is not directly import, but the URI should include a "/", for example:

	/Attack Page

Once the page is created, we reload the site and navigate to the newly created tab on the Home Page. Inside the newly created page, we choose to "edit" the page and turn on "expert" & "enable LSP."

To gain access we attempt to place the Sploitus code with our local host and port combination with a net cat listener and click "save."

With success, we achieve a reverse root shell and find the root flag in the root folder:
		cat /root/proof.txt

listening on [any] 9001 ...
connect to [192.168.xxx.xx] from (UNKNOWN) [192.168.220.25] 35186
pwd
/var/www/html
cd /root && ls -la
total 32
drwx------  3 root root 4096 Sep 12 16:27 .
drwxr-xr-x 18 root root 4096 Jun 13  2023 ..
-rw-------  1 root root   73 Jun 15  2023 .bash_history
-rw-r--r--  1 root root  571 Apr 10  2021 .bashrc
-rw-r--r--  1 root root   21 Jun 14  2023 email4.txt
drwxr-xr-x  3 root root 4096 Jun 13  2023 .local
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
-rw-r--r--  1 root root   33 Sep 12 16:27 proof.txt
pwd
/var/www/html
cat /root/proof.txt
1400207fd92a809c64088b690aee27af

Flag -> ==1400207fd92a809c64088b690aee27af==
# 
















