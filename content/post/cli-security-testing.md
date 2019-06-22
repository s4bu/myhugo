---
title: "CLI Security Testing"
tags: [ "Security Testing", "0-day", "Fuzzing", "CLI"]
date: 2017-12-26
cover: "/media/CommandMyUnknownLord.gif"
---

## Command Line Interface Security Testing

CLIs (Command Line Interface/Utility) offer a lot of commands to make system information easily available & manageable. Many of these commands offer various arguments (functionalities). These command line utilities and their arguments should be programmed in such a way that they should not be vulnerable or contain any logical flaw that can allow malicious user of CLI to escalate privilege, access unauthorized info, bypass ACL etc.
<br>Here are test cases for security testing of command line utilities:

1. Parameter fuzzing
2. Environment variable manipulation
3. Path maniputation
4. Deletion/overwrite of important OS files to cause dos
5. Rootkit attaching via CLI
6. Debug info leakage
7. Memory leaks
8. SetUID & GID
9. Path traversal Direct object reference via parameters
10. OS command injection in parameters
11. Malicious use of Config power
12. LKM(*.ko) integrity and manipulation
13. JAR verifier (Signed/Unsigned)
14. Jar Secrets/Business logic (JD-GUI)



**Parameter Fuzzing**

A fuzzer is a program which injects automatically semi-random data into a program stack in order to inject fault and detect existing bugs. Now lets try to test a sample utility "diskutil" (It works on restricted shell only) parameters for any existing vulnerability like memory corruption, command injection & privilege escalation. 
In order to test this utility first we need to find the the potential attack vectors and then try to fuzz all of them to look for any flaw. In our example case diskutil take one param only so we will fuzz that only.
Please watch the video to find out how easily it came out that this utility is vulnerable to privilege escalation and directory traversal OS attacks with help of automated fuzz test case. 

<script src="https://asciinema.org/a/TqtVwTBPUXd1sMnHO9YJQGsZu.js" id="asciicast-TqtVwTBPUXd1sMnHO9YJQGsZu" async></script>
<p style="text-align:center" >**Video: Parameter Fuzzing**</p>




**Important point to note:**

0. Diskutil in real world works on a very restricted shell that does not exposes all the OS commands to active users. 
1. The console on which the fuzzer is running is showing the output of command (./diskutil +FUZZPayload). These FUZZ payloads are nothing but  specially crafted text for injections. So in case of any anomaly like this diskutil show port detail or any other detail will also be visible on the running console. 
2. For sake of simplicity I have used the payload of privilege escalation, os command injection, shell delimiter bypass & directory traversal only. Though the tester can build his own configuration file and can test for many attacks at once. 
3. Sfuzz is sending all the fuzz payload to bash shell with help of a bind shell running on port 1337. So the fuzz data is hitting a real shell terminal during this run and all the output of that shell is available on the tester's terminal.
4. As by the end of video it became clear that this untility is vulnerable to command injection by looking at the output available on tester console. The same process can also be automated with help of callbacks. 


##### **References**:
1. [GitHub](https://github.com/orgcandman/Simple-Fuzzer)



