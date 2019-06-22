---
title: "CTF: Cracking RSA Encryption"
tags: [ "Security Testing", "0-day", "CTF", "Python"]
date: 2018-05-07
cover: "/media/ctfcult.gif"
---

#### **Crypt: Crack Poor RSA** 

**Challenge:**<br>
N = 58900433780152059829684181006276669633073820320761216330291745734792546625247<br>
C = 56191946659070299323432594589209132754159316947267240359739328886944131258862<br>
e = 65537<br>
Reverse encrypted text C to plain text



Below is my code to crack RSA with given N, C & e. {works on py2+} 

```python
from Crypto.PublicKey import RSA
import gmpy2

def int2Text(number, size):
    text = "".join([chr((number >> j) & 0xff) for j in reversed(range(0, size << 3, 8))])
    return text.lstrip("\x00")

N = 58900433780152059829684181006276669633073820320761216330291745734792546625247
C = 56191946659070299323432594589209132754159316947267240359739328886944131258862
e = 65537L
#http://factordb.com/index.php?query=58900433780152059829684181006276669633073820320761216330291745734792546625247

p = 176773485669509339371361332756951225661L
q = 333197218785800427026869958933009188427L
r=(p-1)*(q-1)
d = long(gmpy2.divm(1, e, r))

rsa = RSA.construct((N,e,d,p,q))
pt = rsa.decrypt(C)

print pt  # returns 6872557977505747778161182217242712228364873860070580111494526546045
print int2Text(pt,100) #returns timctf{th0sE_rOoKIe_numB3rz}


```




##### **References**:
1. [timisoaractf](https://contest.timisoaractf.com/challenges?category=crypto)
