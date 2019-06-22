---
title: "Embed powershell scripts in the pixels of png"
tags: ["Hack", "Powershell", "Exploit"]
date: 2017-12-15
cover: "/media/strange.gif"
---

## Attack

Invoke-PSImage takes a PowerShell script and embeds the bytes of the script into the pixels of a PNG image. It generates a oneliner for executing either from file locally or from web.
This script can be very handy while targetting an memory attack, moving payloads & compromising the system. Antivirus on which I tested failed to recognize the PNG as a malcious file.

![ Virus Total](/media/antivirus-PS.PNG)




## Exploit
**Malicious PNG Generation**

![ Invoke-PSImage](/media/Invoke-PSImage.PNG)


##### **References**:
1. [GitHub](https://github.com/peewpw/Invoke-PSImage) 

