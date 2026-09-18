

# Introduction

This lab exploits a Remote Code Execution (RCE) vulnerability in GLPI 10.0.2 (CVE-2022-35914) to achieve initial access. Attackers escalate privilege by leveraging a writable Jetty server's webapps directory to trigger a reverse shell through crafted XML configuration files. 
# Objective

- Enumerate open services and identify the GLPI 10.0.2 instance running on port 80.
- Exploit the GLPI RCE vulnerability using a crafted payload to obtain a reverse shell as www-data.
- Extract database credentials from the config_db.php file and use them to retrieve user passwords.
- SSH into the target as the betty user using extracted credentials.
- Exploit the writable Jetty server webapps folder to deploy a crafted XML configuration for a reverse shell, escalating to root access.
# Service Enumeration

Nmap Scan results:


# Information Gathering 


# Privilege Escalation