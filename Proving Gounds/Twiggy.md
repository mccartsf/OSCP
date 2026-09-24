# Introduction

This lab exploits a pre-auth remote code execution vulnerability in SaltStack Master (CVE-2020-11651). Attackers will leverage the SaltStack API to execute arbitrary commands, resulting in a root shell on the target. 

# Objective

- Enumerate services and identify the SaltStack API running on ports 4505 and 4506.
- Investigate HTTP headers to determine the SaltStack version and verify vulnerability exposure.
- Deploy the CVE-2020-11651 exploit to execute arbitrary commands on the SaltStack Master.
- Establish a reverse shell and confirm root-level access to the target.
- Understand the importance of securing critical management tools against known vulnerabilities.

# Information Gathering 


# Service Enumeration


# Privilege Escalation