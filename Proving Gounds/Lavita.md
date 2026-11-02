# Information Gathering 

Given the target URL, we will first attempt to see if there is any visible web page available. When visiting the target URL we are met with a consulting website that appears to have a login page for a "Demo" page. 

Registration info:
test@hacket.com
12345678

After registering we land on the demo page which allows us to upload file attachments. In addition to the demo, we can enable or disable debugging mode for more information. 

Since we know the web page is taking file uploads, we can attempt to perform file injection attacks with different code versions.

Unfortunately, after several different file attempts, we are unable to get any debugging information or reverse shell accesses. 

Let's validate the HTTP methods using Burpsuite. 

Using Burpsuite, we notice that when the file is directly uploaded, the subdomain which the GET requests is intended is different from the target IP. Once we remove this domain, we land directly on a debugging redirect page. 

From this page, we can gather information about the target IP and server information. Specifically, we find out that the target IP is running Laravel version 8.4.2.
# Service Enumeration



# Privilege Escalation