
# Introduction

This lab demonstrates exploiting a mass assignment vulnerability in a Rails-based application to confirm user accounts, enabling access to the file manager. Attackers can leverage path traversal through the url cwd parameter to upload an SSH public key into the .ssh/authorized_keys file, enabling remote access as the remi user. Privilege escalation is performed by abusing a pre-configured SSH alias to log in as root. 
# Objective

- Enumerate services to identify a Rails-based web application and paths using dirb.
- Exploit a mass assignment vulnerability in the account confirmation process.
- Leverage the file manager to upload an SSH public key to the .ssh/authorized_keys file.
- Gain SSH access as the remi user using the uploaded private key.
- Use a pre-configured SSH alias with IdentitiesOnly to log in as root and gain full system access.

# Information Gathering 

Nmap Scan results:

Starting Nmap 7.99 ( https://nmap.org ) at 2026-10-03 18:02 -0400
NSE: Loaded 158 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 18:02
Completed NSE at 18:02, 0.00s elapsed
Initiating NSE at 18:02
Completed NSE at 18:02, 0.00s elapsed
Initiating NSE at 18:02
Completed NSE at 18:02, 0.00s elapsed
Initiating Ping Scan at 18:02
Scanning 192.168.238.231 [4 ports]
Completed Ping Scan at 18:02, 0.17s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 18:02
Completed Parallel DNS resolution of 1 host. at 18:02, 0.50s elapsed
Initiating SYN Stealth Scan at 18:02
Scanning 192.168.238.231 [65535 ports]
Discovered open port 22/tcp on 192.168.238.231
Discovered open port 80/tcp on 192.168.238.231

Gobuster Results:

login                (Status: 200) [Size: 2413]
register             (Status: 200) [Size: 2765]
404                  (Status: 200) [Size: 1722]
500                  (Status: 200) [Size: 1635]
422                  (Status: 200) [Size: 1705]
filemanager          (Status: 302) [Size: 94] [--> http://192.168.238.231/login]
Progress: 100000 / 100000 (100.00%)

The target IP address will open up to a "Boolean" login page, but no other subdomains are available. We are going to try creating an account and navigating through software after successful login.
# Service Enumeration

On the login page, I have created the user:

john1
1234

With this username/password combination, I am able to access the homepage of the Boolean website, but am prompted to confirm my email address using the email on file. After multiple attempts, I am unable to get the email to be received on multiple email accounts, leading to direct manipulation through Burpsuite.

Using Burpsuite, we can attempt to trick the web page into thinking the currently logged-in user is writing from an already "confirmed" account. To perform this, we capture the HTTP POST request being sent out and modify the user tags at the bottom of the request:

	user%5B%5d -> user%5Bconfirmed%5D

By adding the word "confirmed" in the request, we can get a response back from the server that accepts our currently logged-in user as already having confirmed themselves via email.

Now that we have successfully authenticated the account, we are redirected to the /filemanager subdomain which allows the ability to directly upload files.

When uploading a test.txt file to the web application we notice that the file gets directly downloaded to the website and the url changes significantly.

For example, prior to uploading the file, the url read:

	http://192.168.238.231/filemanager

However, after we upload a file it appears as:

	http://192.168.238.231/?cwd=file=test.txt&download=true

Noticing this, we can attempt to perform a directory transversal attack on the "cwd" parameter which appears to stand for "current working directory." Using this we place the /etc directory in the cwd paremeter, "passwd" in the "file=" parameter, and work our way through the directory until we achieve a working download of the passwd file.

With success, we achieve a download of the "passwd" file.

Combing through the file we find a user that goes by the name of "remi" which could be used as a potential foothold for our target IP.

Since we have directory viewing and download privileges on the machine we can try to access the "home" directory for any information on other users. 

In the "home" directory we can find a directory named, "remi" that contains files and directories related to SSH login keys. 

After downloading these files and attempting to login with these public/private keys using the "remi" account, there was no successful foothold. 

However, since we have the ability to upload directly to the file directory which contains public private key combinations, we can create out own key pair and try to bypass the server authentication this way. 

For example, we will create a key pair, upload the files as "authorized_keys", and then attempt a login as "remi" using the private key which we created.

	1) ssh-keygen -q -N '' -f sshkey
	2) mv sshkey.pub authorized_keys
	3) ssh -i sshkey  remi@192.168.238.231

With success, we are able to get a foothold on the target machine as the "remi" user!

The local flag can be found in the home directory.
# Privilege Escalation
