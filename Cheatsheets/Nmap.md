
scan all ports (-p-), do version detection (-sV), script mode (-sC), with verbose output (-v)

	nmap -sC -sV -v -p-

## UDP Scans 

	nmap -p 53,67,68,69,111,123,161,162,137,138,139,514,1900,5353,500,445 -sU <IPADDRESS>