---
title: "Hacking JSON Web Token"
tags: ["Web Attack", "NodeJS", "Exploit", "Brute Force", "Web Exploit"]
date: 2017-12-06
cover: "/media/jwt-token.png"
---

## Attack

JWT is a URL safe, stateless protocol for transferring claims. A JWT token looks something like this:
**<span style="background-color: #98FB98">Header</span>.<span style="background-color: 	#87CEFA">UserStateInformation</span>.<span style="background-color: #FF0000">Signature</span>**

Sample:<br>
<span style="background-color: #98FB98">eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.<br></span>
<span style="background-color: 	#87CEFA">eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.<br></span>
<span style="background-color: #FF0000">TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ</span><br>

The information in token are separated by dots. The first and second part can be easily converted to ascii as they are base64 encoding of plain text. That being said lets dig in to these three parts of JWT token, header contain information about the algo used to encrypt (correct term would be hash generation :P). In this case HMACSHA256 is used to generate the signature out of header+UserStateInfo+Secret.

**Sample JWT Generation**

```python
>>> import jwt
>>> encoded = jwt.encode({'some': 'payload'}, 'secret', algorithm='HS256')
'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzb21lIjoicGF5bG9hZCJ9.4twFt5NiznN84AWoo1d7KO1T_yoc0Z6XOpOVswacPZg'

>>> jwt.decode(encoded, 'secret', algorithms=['HS256'])
{'some': 'payload'
```

So by now it will be clear that we are after this secret because once it is known to anyone he can generate JWT token and can hijack user sessions. The JWT token authentication system is based on verifying this claim only that the token received from the client is having correct signature as per the secret hardcoded in the backend web application. 
There are some ways by which you can set even expiration date on these token like in UserStateInformation developer can embedd the timestamp parameter and while validating the message authentication code it can set condition that token will only be fully validated if it has correct MAC and the time mentioned in data section is not day or two old. Though this will be only helpful where there is any chance of social engineering, MITM and physical access to victims system, in case token does not carry any expiry and it persist post logout.



## Exploit

![ Quadratic Blowup Attack GIF](/media/hs256Cracking.gif)


### Secure Implementation

1. Large Large Very Large Secret (Don't forget to make it complex)
2. Expiry Logic (One mentioned in article)
3. Destroying token post login
4. Secure channel transmission (SSL/TLS)
5. Rotating secret after fixed period (Code small function)  
6. Use this auth/claim token as custom header and with proper CORS implementation developer can skip CSRF protection implementation as this will be suffice to protect against that as well.

##### **References**:
1. [JWT](https://jwt.io)
2. [GitHub](https://github.com/lmammino/jwt-cracker) 

