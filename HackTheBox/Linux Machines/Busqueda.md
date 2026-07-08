

#### About

"Busqueda is an Easy Difficulty Linux machine that involves exploiting a command injection vulnerability present in a `Python` module. By leveraging this vulnerability, we gain user-level access to the machine. To escalate privileges to `root`, we discover credentials within a `Git` config file, allowing us to log into a local `Gitea` service. Additionally, we uncover that a system checkup script can be executed with `root` privileges by a specific user. By utilizing this script, we enumerate `Docker` containers that reveal credentials for the `administrator` user&amp;#039;s `Gitea` account. Further analysis of the system checkup script&amp;#039;s source code in a `Git` repository reveals a means to exploit a relative path reference, granting us Remote Code Execution (RCE) with `root` privileges."



Write Up of Actions Taken

1)  How many TCP ports are open on the remote host?

nmap -sT 10.129.228.217

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http


ANSWER: 2

2) Which Python library/command line application is the web-application using to generate URLs?

- Checked the available IP Address on Firefox, but was unable to access the site

- Need to add the target IP Address to "/etc/hosts" hosts on our local machine:

	echo 10.10.11.208 searcher.htb | sudo tee -a /etc/hosts



After the target IP was added to the list, the IP Address is now showing the "Searcher Site."










