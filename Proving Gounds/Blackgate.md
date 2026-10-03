# Introduction

This lab leverages an insecure Redis service to gain initial shell access by deploying a rogue server exploit for remote code execution. Privilege escalation is achieved by exploiting a vulnerable redis-status binary using Return Oriented Programming (ROP) to bypass protections and execute arbitrary commands as root. 
# Objective

- Enumerate open services to identify the Redis server.
- Deploy a rogue Redis server exploit to gain initial access.
- Analyze the redis-status binary to identify a buffer overflow vulnerability.
- Gain root access through privilege escalation

# Information Gathering 


# Service Enumeration


# Privilege Escalation

Auth Key found in the /usr/...redis-status file:

ClimbingParrotKickingDonkey321
ls
