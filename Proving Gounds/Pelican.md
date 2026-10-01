
# Introduction

This lab demonstrates exploiting an unauthenticated command injection vulnerability in the Exhibitor UI for Apache Zookeeper to gain initial access. Attackers will escalate privileges by dumping the memory of a root process using the sudo-allowed gcore command to retrieve root credentials. 

# Objective

- Identify and exploit the Exhibitor UI command injection vulnerability to gain a low-privilege shell.
- Enumerate processes and privileges available to the compromised user.
- Use sudo access to gcore to dump memory of an active root process.
- Analyze the dumped memory to extract sensitive information, such as root credentials.
- Escalate privileges to root using the extracted credentials and validate full system access.

# Information Gathering 


# Service Enumeration


# Privilege Escalation