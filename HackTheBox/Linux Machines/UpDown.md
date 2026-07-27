
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
- GoBuster
- Git-Dumper

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

Once we add this domain to the hosts file, we open the website and visit the main site. 

Using Ffuf, we can FUZZ the "siteisup.htb" domain for additional subdomains within the original target IP address:

		ffuf -w /tmp/test.txt -u http://siteisup.htb -H "Host: FUZZ.siteisup.htb" -fs 1131


This shows us that there is an active subdomain "dev":
![](Pasted%20image%2020260727105304.png)

We then add this subdomain to our /etc/hosts files:

		echo 10.129.30.222 dev.siteisup.htb |sudo tee -a /etc/hosts


# Enumeration

Now that we know there is another available subdomain, we are going to use Gobuster to validate if there are any other hidden directories within the domain:

		gobuster dir -u http://siteisup.htb -w /usr/share/wordlists/dirb/common.txt

Gobuster only reveals the originally discovered, "dev" subdomain. We can try moving one step further investigating this subdomain with the same command:

		gobuster dir -u http://siteisup.htb/dev -w /usr/share/wordlists/dirb/common.txt

A deeper dive into the "dev" subdomain reveals a ".git" subdomain with attached directories and files:

![](Pasted%20image%2020260727112301.png)


To gain a better view of the files we can import them into our linux machine using Git-Dumper:

		git-dumper http://siteisup.htb/dev/.git dev

After some analyzing of files and hidden files within the "dev" directory, we can find a file named, ".htaccess" that appears to grant "Special-Dev" access if "only4dev" is placed in a required header

.htaccess file content:

SetEnvIfNoCase Special-Dev "only4dev" Required-Header
Order Deny, Allow
Deny from All
Allow from env=Required-Header


# Foothold


# Privilege Escalation


## HTB Question Checklist














