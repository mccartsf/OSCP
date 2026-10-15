
# Introduction

In this lab, attackers manually exploit a SQL injection to obtain credentials and leverage write access to a user's filesystem for SSH access. Attackers then analyze an interesting ELF binary to discover a wildcard bug that facilitates privilege escalation to root access. 
# Objective

- Perform enumeration to identify a vulnerable login page.
- Exploit SQL Injection to bypass login authentication and extract credentials.
- Use the extracted credentials to gain access to the system via SSH.
- Analyze the AuthChecker SUID binary and identify the wildcard bug in its cp command.
# Information Gathering 


# Service Enumeration


# Privilege Escalation


