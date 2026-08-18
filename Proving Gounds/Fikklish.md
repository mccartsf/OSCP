

# About

This lab demonstrates exploiting a Remote Code Execution (RCE) vulnerability in Weblate 4.11 to gain a foothold on the target system. Learners escalate privileges by abusing an argument injection vulnerability in the git RubyGem (v1.10.2) within a custom Ruby script executed with elevated privileges. This lab highlights web application exploitation, credential discovery, and argument injection for privilege escalation.

## Skills Being Worked On

A vulnerability in Weblate 4.11 will be exploited to establish an initial foothold. Privilege escalation will be performed by thoroughly enumerating the lab to discover a set of credentials and exploiting an argument injection in Git v1.10.2. This lab focuses on exploiting vulnerabilities and privilege escalation methods.

## Skills Improved 

Privilege Escalation
- Remote Code Execution
- Credential Discovery'
- Web App Exploitation
## Used Tools 

- Nmap 

# Information Gathering 

Nmap Scans:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-17 10:53 -0400
Nmap scan report for 192.168.119.19
Host is up (0.071s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  closed https
8000/tcp open   http-alt


## HTTP

Port 80 is open and running a service, we can check on the IP address listed to see if the website is active.

After checking the website IP Address, we find the address is active and opens to a online bookshop:

Port 80:

![](Pasted%20image%2020260817095935.png)


Port 8000:


# Enumeration

Knowing that the IP Address is up and available we are going to search around on the website for more available vulnerability information. 

gobuster results:

images               (Status: 301) [Size: 317] [--> http://192.168.119.19/images/]
js                   (Status: 301) [Size: 313] [--> http://192.168.119.19/js/]
css                  (Status: 301) [Size: 314] [--> http://192.168.119.19/css/]
fonts                (Status: 301) [Size: 316] [--> http://192.168.119.19/fonts/]
javascript           (Status: 301) [Size: 321] [--> http://192.168.119.19/javascript/]
sass                 (Status: 301) [Size: 315] [--> http://192.168.119.19/sass/]
Progress: 100000 / 100000 (100.00%)

After searching 100,000 subdomains, only a few HTML/CSS related subdomains were found. Each subdomain is individually related to website design

gobuster results from searching on port 8000:

admin                (Status: 301) [Size: 0] [--> /admin/]
stats                (Status: 301) [Size: 0] [--> /stats/]
search               (Status: 301) [Size: 0] [--> /search/]
data                 (Status: 301) [Size: 0] [--> /data/]
hosting              (Status: 301) [Size: 0] [--> /hosting/]
projects             (Status: 301) [Size: 0] [--> /projects/]
manage               (Status: 301) [Size: 0] [--> /manage/]
user                 (Status: 301) [Size: 0] [--> /user/]
about                (Status: 301) [Size: 0] [--> /about/]
contact              (Status: 301) [Size: 0] [--> /contact/]
keys                 (Status: 301) [Size: 0] [--> /keys/]
widgets              (Status: 301) [Size: 0] [--> /widgets/]
trial                (Status: 301) [Size: 0] [--> /trial/]
memory               (Status: 301) [Size: 0] [--> /memory/]


After moving through each of the specific subdomains, we learn that the online web page has an "admin" login site. With some social engineering, trial and error, and brute force, I was able to find a login combination that worked for the admin account:

Admin 
niffenegger

# Foothold

Now that we are logged into the 
# Privilege Escalation


## HTB Question Checklist














