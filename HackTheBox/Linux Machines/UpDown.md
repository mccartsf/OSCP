

# About

UpDown is a medium difficulty Linux machine with SSH and Apache servers exposed. On the Apache server a web application is featured that allows users to check if a webpage is up. A directory named `.git` is identified on the server and can be downloaded to reveal the source code of the `dev` subdomain running on the target, which can only be accessed with a special `HTTP` header. Furthermore, the subdomain allows files to be uploaded, leading to remote code execution using the `phar://` PHP wrapper. The Pivot consists of injecting code into a `SUID` `Python` script and obtaining a shell as the `developer` user, who may run `easy_install` with `Sudo`, without a password. This can be leveraged by creating a malicious python script and running `easy_install` on it, as the elevated privileges are not dropped, allowing us to maintain access as `root`.
## Skills Being Worked On

- Web Enumeration 
- Local Git repository hacking 
- PHP File Inclusion

## Skills Improved 

- HTTP Header modification 
- PHP Local File Inclusion Firewall bypass 
- Exploiting SUID binaries

## Used Tools 

- Nmap
- Ffuf

# Information Gathering 

Scanned all available TCP ports:

nmap -sT 10.129.227.227
Starting Nmap 7.95 ( https://nmap.org ) at 2026-07-23 12:28 EDT
Nmap scan report for 10.129.227.227
Host is up (0.0078s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

## HTTP
The target IP address opens to a, "Is My Website Up?" domain that checks to see if domains are up or not.

For reference, we needed to manually map the hostname to the target IP address:

		echo siteisup.htb 10.129.227.227 | sudo tee -a 10.129.227.227 /etc/hosts

# Enumeration



# Foothold


# Privilege Escalation


## HTB Question Checklist














