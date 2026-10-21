
# Information Gathering 

Nmap Scans:

No scan performed

Gobuster:

Not Performed


While visiting the target IP address, the landing page reads as a "ImageMagik Identifier" prompting us to upload a file to identify. 

To test the functionality of the website, we upload a test.txt file for better results.

Result:

	File uploaded successfully
	Version: 6.9.6-4

Using this information we runs a quick search for known vulnerabilities of "ImageMagik version 6.9.6-4." Luckily, there is a known vulnerability of this software under [CVE-2023-34152](https://github.com/SudoIndividual/CVE-2023-34152/tree/main).

# Service Enumeration

Under [CVE-2023-34152](https://github.com/SudoIndividual/CVE-2023-34152/tree/main), we can find a proof of concept at this publicly available GitHub -> 
https://github.com/SudoIndividual/CVE-2023-34152/blob/main/CVE-2023-34152.py

*More details about the exploit can be found directly on the Github, or in the mentioned CVE*

To use this proof of concept, we run the below command

	python3 CVE-2023-34152.py 192.168.XXX.XXX 80

This will create an uploadable payload to the ImageMagik website creating a reverse shell back to your local machine.

After uploading the payload, the website pauses and we receive a shell on our local machine!

The "local.txt" flag can be found in the home directory.
# Privilege Escalation

