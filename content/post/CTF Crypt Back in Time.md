---
title: "CTF: Back in Time"
tags: [ "Security Testing", "0-day", "CTF", "Python"]
date: 2018-04-16
cover: "/media/ctfcult.gif"
---

#### **Crypt: Back in Time** 

**Challenge:**<br>
I always hated history class. I thought history would never come in handy.
With challenge there are two files:

1: [encrypt.py](https://contest.timisoaractf.com/download?file_key=8c9f88ff278683172b69aadd6178b85f5129bee1c98d7cfc68027d50717e6c2c&team_key=09c8bc7fb0075cb2c6fe373a06016df5aece27dd94b1cf8374eb35b492426797) 

2: [cipheretext.txt](https://contest.timisoaractf.com/download?file_key=5b17f2c304a6bd25dc671b66088aaf65be8be1fd0893e33a56889d8990bb5cad&team_key=09c8bc7fb0075cb2c6fe373a06016df5aece27dd94b1cf8374eb35b492426797)


Below is the content of encrypt.py file 

```python
import random

alpha = "abcdefghijklmnopqrstuvwxyz"
key = ''.join(random.sample(alpha,len(alpha)))
print key
assert(len(alpha) == 26)

plaintext = open("plaintext.txt").read()
ciphertext = ""

sub_dict = {}

for i in range(len(alpha)):
    sub_dict[alpha[i]] = key[i]

for i in range(len(plaintext)):
    if plaintext[i] in alpha:
        ciphertext += sub_dict[plaintext[i]]
    else:
        ciphertext += plaintext[i]

open("ciphertext.txt", "w").write(ciphertext)

```


Below is the content of cipheretext.text:

```text
hsijhk{Pc3nvO_R4NvwM_1S_Nwh_RArD0M}
```

Now as it is very clear that key is generated using random function seed. Now the only way to decrypt the content is find the actual key used for encryption. Now as it is already known that all flag will follow the following format:

timctf{fl4g_is_h3r3}

So now we have our known text in encrypted data and can use it to find random sub_dict/key and perform known plain text attack.
Now in order to do that we need one script which can find the dictionary used for encryption. I create following python script using asyncio to find sub_dict used during encryption. Asyncio helps to improve the perfromance with killing my system so I give it try and it work out very well. So in program I am storing all such sub_dict in a file so that I can use them to apply decrypt logic.


```python
#CTF: Back in Time
#Find/Brute key dictionary based on choosen known plain text timctf
import random
import sys,os
import time
import asyncio
glueKey=0
keydic = open("C:\\Users\\aLt\\Downloads\\key.txt",'w')
async def run():
    global keydic
    global glueKey
    alpha = "abcdefghijklmnopqrstuvwxyz"
    assert(len(alpha) == 26)
    ciphertext = "hsijhk"
    plaintext = ""
    sub_dict = {}
    while 1:
        key = ''.join(random.sample(alpha,len(alpha)))
        for i in range(len(alpha)):
            sub_dict[alpha[i]] = key[i]

        for i in range(len(ciphertext)):
            if ciphertext[i] in alpha:
                plaintext += sub_dict[ciphertext[i]]
            else:
                plaintext += ciphertext[i]
        print("plain text value and key found: ",plaintext, glueKey)
        asyncio.sleep(0.25)
        if plaintext=="timctf":
            asyncio.sleep(0.50)
            print(sub_dict)
            keydic.write(str(sub_dict) + '\n')
            keydic.flush()
            glueKey+=1
            print(glueKey)
            if glueKey >= 20:
                sys.exit(0)
        plaintext = ""

if __name__=="__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(run())
    keydic.close()


```


So with help of above script with 2hr I was able to find 20 possible key dictionaries. Now its time to reverse the encrypt logic and build the decrypt logic and supply all keys found.



```python
#CTF: Back in Time
#Crack encrypted flag with key dictionary from above script
import random
import json
import pprint
import ast
alpha = "abcdefghijklmnopqrstuvwxyz"
key = ''.join(random.sample(alpha,len(alpha)))
assert(len(alpha) == 26)

plaintext = ""
ciphertext = "Pc3nvO_R4NvwM_1S_Nwh_RArD0M"
data=[]
with open("C:\\Users\\aLt\\Downloads\\key.txt") as keyfp:
    for line in keyfp:
        data.append(ast.literal_eval(line))

for sub_dict in data:
    for i in range(len(ciphertext)):
        if ciphertext[i] in alpha:
            plaintext += sub_dict[ciphertext[i]]
        else:
            plaintext += ciphertext[i]
    print("Cracked Plain test: ",plaintext)  
    plaintext = ""


```  


Finally you have will ~ decrypted content. 




##### **References**:
1. [timisoaractf](https://contest.timisoaractf.com/challenges?category=crypto)
