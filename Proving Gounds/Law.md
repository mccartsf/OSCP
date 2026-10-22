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

To perform the exploit on the machine we need to follow the proof of concept found on Mayfly's Github (credit to author) -> https://mayfly277.github.io/posts/GLPI-htmlawed-CVE-2022-35914/

This detailed explanation describes a method of attack that makes use of command injection within the htLawed function due to the similar format with the php "exec" command format. 

In this machine, the only diffference we need to make when running the exploit is that directed IP address when adding our commands to the website. 

When clicking on the "process" button, the website directly tries to attempt and send the GET request to the http://192.168.XXX.XXX/htmLawedTest.php  site. However, this request need to be intercepted with Burp Suite and re-routed to http://192.168.XXX.XXX/. 

With this, we are able to post a reverse shell command and land a foothold on the target machine.

The local flag can be found in the user's home directory. 

# Privilege Escalation
