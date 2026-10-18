# Introduction


# Objective


# Information Gathering 

Nmap Scans:

No readable results found

Gobuster Results:

===============================================================  
wordpress            (Status: 301) [Size: 320] [--> http://192.168.224.16/wordpress/]  
filemanager          (Status: 301) [Size: 322] [--> http://192.168.224.16/filemanager/]  
Progress: 100000 / 100000 (100.00%)


Let's check the filemanager subdomain. This subdomain opens to a login page with no version information available. However, the site is weakly protected with the common login credential 
"admin admin." 

Now that we have access, we see a page that appears to be a directory management system of files for the system. Let's review the files and see if there is any useful information that we can pull from the files.

*Useful Information:*
User names and password found in the /filmanager/config/htusers.php file

U - admin / P - 21232f297a57a5a743894a0e4a801fc3
U - dora / P - $2a$08$zyiNvVoP/UuSMgO2rKDtLuox.vYj.3hZPVYq3i4oG3/CtgET7CjjS

The "dora" account password shows us  a hash that can be cracked using John the Ripper:

	john hash.txt --wordlists=/usr/share/wordlists/rockyou.txt


After viewing all of the files in the filemanager, we learn in the changelog.txt file that the currently running version of EXtplorer is at least 2.1.15. Since we know the version number we can research available exploits on the web.

# Service Enumeration



# Privilege Escalation

