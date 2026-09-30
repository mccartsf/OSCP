
## Information Gathering

- [ ] Run available Nmap scans on the $targetIP 
- [ ] Review all running ports available in the scans and check for services
- [ ] Check the $targetIP address in web browser for connection
	- [ ] If not connection, add the $targetIP to the /etc/hosts file and retry
	- [ ] If still no connection, try accessing the $targetIP using a different port
- [ ] Check Gobuster for available and hidden subdomains
	- [ ] Investigate the found subdomains 
	- [ ] Are there any login pages? 
	- [ ] Is there any website software versions information available? 
	- [ ] Does the subdomain contain any *config* files or directories? 

## Enumeration

- [ ] With the available port information, run commands in the "available port" cheat sheet for any available connections
- [ ] This is a test



