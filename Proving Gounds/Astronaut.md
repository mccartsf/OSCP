
# Introduction

This lab exploits an unauthenticated arbitrary YAML write/update vulnerability in Grav CMS to achieve Remote Code Execution (RCE). Attackers escalate privileges by leveraging a SUID-enabled php7.4 binary, allowing command execution as root. 

# Objective

- Enumerate open ports and identify Grav CMS running on port 80.
- Exploit the YAML write vulnerability (CVE-2021-21425) to achieve Remote Code Execution and gain a shell as www-data.
- Enumerate SUID binaries and identify /usr/bin/php7.4 as a potential privilege escalation vector.
- Use pcntl_exec() with the SUID-enabled PHP binary to spawn a root shell.
- Verify root-level access and explore the compromised system.


# Information Gathering 

Nmap Sca
# Service Enumeration 



# Privilege Escalation

















