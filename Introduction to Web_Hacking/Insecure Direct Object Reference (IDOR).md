- Insecure Direct Object Reference (IDOR) 
	- Access Control Vulnerability
	- Occurs when a web server receives user-supplied input to retrieve objects (files, data, documents)

Example:
In an online store, we are able to see our order history and receipts for past orders. The below link allows us to see the invoice for order # 1234

Starting URL -> https://onlinestore.thm/order/1234/invoice

New URL -> https://onlinestore.thm/order/1000/invoice

^
In the above example, we have changed the value of "1234" to "1000" to see if we could indirectly access the invoice to other orders on the site. To our surprise, we are able to access order "1000" in the new URL. 

- In this example, no validation was made by the server-side (web server) to check if the people requesting the object owned the object


### Encoded IDs

 When passing data from page to page either by post data, query strings, or cookies, web developers will often first take the raw data and encode it.
  
  - Encoding this data changes binary data into ASCII strings commonly using the `a-z, A-Z, 0-9 and =` characters for padding
  - The most common encoding method on the web is ==base64== encoding 

Base64 Decoder ->  [https://www.base64decode.org/](https://www.base64decode.org/)

Basae64 Encoder -> https://www.base64encode.org/

### Hashed IDs

Hashed IDs work like encoded IDs, but are more complicated in decoding. 
- Hashed values are often integer values such as 123
- MD5 is a common hashing encryption used to create hashes
- Discovered hashes can sometimes be uncovered by crack station (database of known hash values to results)

Ex. https://crackstation.net/

### Unpredictable IDs 

If the Id cannot be detected using the above methods, an excellent method of IDOR detection is to create two accounts and swap the Id numbers between them. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all), you've found a valid IDOR vulnerability.

### Where are IDORs located? 
- May not always be something you see in the address bar; It could be content your browser loads in via an AJAX request or something that you find referenced in a JavaScript file.

- Sometimes endpoints can have an unreferenced parameter that may have been of some use during development and got pushed to production. 

Ex. 
You may notice a call to **/user/details** displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called **user_id** that you can use to display other users' information, for example, **/user/details?user_id=123**.

