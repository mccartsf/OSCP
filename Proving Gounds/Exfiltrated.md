
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


# Service Enumeration


# Privilege Escalation