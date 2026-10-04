
# Introduction

This lab demonstrates exploiting a mass assignment vulnerability in a Rails-based application to confirm user accounts, enabling access to the file manager. Attackers can leverage path traversal through the url cwd parameter to upload an SSH public key into the .ssh/authorized_keys file, enabling remote access as the remi user. Privilege escalation is performed by abusing a pre-configured SSH alias to log in as root. 
# Objective

- Enumerate services to identify a Rails-based web application and paths using dirb.
- Exploit a mass assignment vulnerability in the account confirmation process.
- Leverage the file manager to upload an SSH public key to the .ssh/authorized_keys file.
- Gain SSH access as the remi user using the uploaded private key.
- Use a pre-configured SSH alias with IdentitiesOnly to log in as root and gain full system access.

# Information Gathering 

username: john1
password: 1234

# Service Enumeration

http://192.168.238.231/?cwd=../../../../../../../home/remi/.ssh/keys&file=id_rsa&download=true

# Privilege Escalation
