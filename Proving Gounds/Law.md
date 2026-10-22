# Information Gathering 

Nmap Scans:
Starting Nmap 7.99 ( https://nmap.org ) at 2026-10-21 22:05 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
Initiating NSE at 22:05
Completed NSE at 22:05, 0.00s elapsed
Initiating Ping Scan at 22:05
Scanning 192.168.165.190 [4 ports]
Completed Ping Scan at 22:05, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 22:05
Completed Parallel DNS resolution of 1 host. at 22:05, 0.55s elapsed
Initiating SYN Stealth Scan at 22:05
Scanning 192.168.165.190 [65535 ports]
Discovered open port 80/tcp on 192.168.165.190
Discovered open port 22/tcp on 192.168.165.190
SYN Stealth Scan Timing: About 28.04% done; ETC: 22:07 (0:01:20 remaining)

==🟡Available ports 22, 80==

Gobuster Results:

No subdomains found.

When accessing the target IP address we are landed on a "HTMLawed 1.2.5 Test" page. If you have performed some of TJ Null's recommended OSCP boxes, you may have seen a similar format in the "GLPI" box. 

However, we will treat this target IP as any other machine. 

Knowing the available version information about the website, we can research any known vulnerabilities for the target IP version number.

Luckily, there is a known vulnerability,  CVE-2022-35914, that can help with exploitation of the machine.

# Service Enumeration


# Privilege Escalation
