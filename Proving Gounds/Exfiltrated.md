
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

Once the IP address was added to the hosts file, we are able to acces the front page of a "kickstart" web page.

Upon inspection there appears to be a Admin Login page on this site that is powered by Subrion CMS v 4.2.1, we can verify if there are any exploits in Metasploit or SearchSploit.


According to searchsploit there is a remote execution code exploit that is prompted by a fiel upload on the kickstart dashboard site. We will use this to gain an initial foothold in the system.

# Service Enumeration





# Privilege Escalation