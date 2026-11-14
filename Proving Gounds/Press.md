# Information Gathering 
Nmap Scans:
PORT     STATE SERVICE VERSION  
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)  
| ssh-hostkey:    
|   3072 c9:c3:da:15:28:3b:f1:f8:9a:36:df:4d:36:6b:a7:44 (RSA)  
|   256 26:03:2b:f6:da:90:1d:1b:ec:8d:8f:8d:1e:7e:3d:6b (ECDSA)  
|_  256 fb:43:b2:b0:19:2f:d3:f6:bc:aa:60:67:ab:c1:af:37 (ED25519)  
80/tcp   open  http    Apache httpd 2.4.56 ((Debian))  
| http-methods:    
|_  Supported Methods: GET POST OPTIONS HEAD  
|_http-title: Lugx Gaming Shop HTML5 Template  
|_http-server-header: Apache/2.4.56 (Debian)  
8089/tcp open  http    Apache httpd 2.4.56 ((Debian))  
| http-methods:    
|_  Supported Methods: GET HEAD POST OPTIONS  
|_http-favicon: Unknown favicon MD5: 315957B26C1BD8805590E36985990754  
|_http-title: FlatPress  
|_http-server-header: Apache/2.4.56 (Debian)  
|_http-generator: FlatPress fp-1.2.1  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

==🟡Available ports 22,80,8089 ==

Hydra Scans:
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-11-13 20:45:31  

login: admin   password: 123456  
login: admin   password: princess  
login: admin   password: abc123  
login: admin   password: 12345  
login: admin   password: nicole  
login: admin   password: 1234567  
login: admin   password: 12345678  
login: admin   password: 123456789  
login: admin   password: iloveyou  
login: admin   password: lovely  
login: admin   password: jessica  
login: admin   password: rockyou  
login: admin   password: daniel  
login: admin   password: babygirl

Numerous password login credentials found for login. 

# Service Enumeration



# Privilege Escalation
