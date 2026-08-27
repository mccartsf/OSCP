
# About

This lab demonstrates exploiting a file read vulnerability in mPDF 6.0 to leak sensitive configuration files, such as config.php, containing database credentials. Using these credentials, learners gain SSH access to the server. Privilege escalation is achieved by exploiting a Remote Code Execution (RCE) vulnerability in Webmin 1.996, granting root-level access. This lab highlights chained web application vulnerabilities and leveraging outdated administrative tools for privilege escalation.

## Skills Being Worked On

An mPDF vulnerability in version 6.0 will be exploited to obtain an initial foothold. Privileges will then be escalated by exploiting a vulnerability in an outdated version of Webmin 1.996. This lab focuses on exploiting application vulnerabilities and privilege escalation methods.


## Used Tools 

- Nmap

# Information Gathering 

Nmap Scans:

nmap -sT 192.168.142.232
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-26 20:59 -0400
Nmap scan report for 192.168.142.232
Host is up (0.041s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
10000/tcp filtered snet-sensor-mgmt

map -sA 192.168.142.232
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-26 20:59 -0400
Nmap scan report for 192.168.142.232
Host is up (0.055s latency).
Not shown: 999 unfiltered tcp ports (reset)
PORT      STATE    SERVICE
10000/tcp filtered snet-sensor-mgmt



# Foothold


# Privilege Escalation

















