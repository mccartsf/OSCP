

# Introduction

This lab will demonstrate a Remote Code Execution (RCE) vulnerability in the PDFKit gem (CVE-2022-25765) used in a RubyDome HTML-to-PDF conversion service. Privilege escalation is leverage by a sudo misconfiguration allowing the user "andrew" to execute a Ruby script (app.rb) with root privileges. By injecting a malicious payload into the script, the attacker is able gain a root shell. 
# Objective

- Enumerate open ports to identify the RubyDome service running on port 3000.
- Exploit the PDFKit RCE vulnerability (CVE-2022-25765) to achieve a reverse shell 
- Enumerate sudo privileges for the andrew user to discover executable Ruby scripts.
- Modify the app.b script to inject a malicious payload for command execution.
- Execute the script using sudo and escalate privileges to root. 


# Information Gathering 



# Service Enumeration 



# Privilege Escalation

















