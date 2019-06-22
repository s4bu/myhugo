---
title: Agile Web Security Automation
tags: ["Web Attack", "Security Automation", "Hack", "Web Exploit"]
date: 2018-05-23
cover: "/media/swaggerSecurityTestingAutomation-1.gif"
---

## Agile Web Security Automation

Automation of application security scans are becoming very common these days. With the advent of devops security and appsec pipeline tools, it has made easy to manage, maintain and scale activities like job scheduling, report generalization from different tool, and SDL integration.
Such pipelines once developed requires minimum intervention until the reports are ready and triage needs to be done after that only. 
In order to maintain a good balance of unique applications and result quality of scanners it is very import to run automations setup and their health check following agile methodolgy. This not only helps in improving the over all pipeline and results but also helps to find out the gray areas which are not getting through the scanner radar.

So here is the board for appsec team for pipeline automation and you can relate to kanban board because it get used in somewhat similar manner.  


| Automation Jobs   |Analysis of completed Scans |  Analysis of completed Scans |
|---|---|---|
|  job(type:web scan, targets:10) |  job(type:web scan, targets:2) |  push(web scan, GWT scripts) |
|   job(type:network scan,targets:20) | job(type:network scan,targets:5)  |   push(network scan, nasl script) |
| job(type:malware scan,targets:4)  |  job(type:network scan,targets:5) |  push(static code, rules) |
| job(type:malware scan,targets:4)  | job(type:system/local scan,targets:7)  |  push(DAST/SAST, hooks) |
|  job(type:code scan,targets:40) | job(type:code scan,targets:10)  |   |

Now comes the important question "how to do analysis"? well in order to reach there you need to write some custom code or start digging into logs at first to start.
For example, for appscan enterprise you can write you custom script to do the coverage analysis and scan error dump. You can run this script on the scan log to get the desired output. 
So the analysis starts with hooking in to the scanner or target. 

## The Hooks 
Hooks can be a middleware layer to monitor the scanner pattern and specific error responses in order to faciliatate the analysis later. Like in case if you web service is running on apache and mod_Security has set some limit of dos/ddos than its high possible that your scanner failed penetrate the surface of real application and ended scaning the error page :D, there hooks become very helpful in easily investigating the real issues.
There are lots of similar exceptional cases in DAST, SAST scanners where do not perform well when you target many different applications. In such situation hooks on the application and scanners helps out to identify such grey areas during the completed scan analysis. 


## Descriptive analysis of Security Test case 
In most of the case the report ends up show critical, high, medium and low etc. what if the scanner was really high :D during scanning and some of the inbuilt plugins fails to workout ?
It is always better to get entire dump of what all test cases ran from the scanners and which plugin/script resulted what ?
This can be done in most of the scanners(DAST and SAST) with one or another way. Some requires custom scripts, extension or little bit of reversing.
Here is example descriptive dump of all test case that I populate automatically post every scan:
![ Security Test cases](/media/enhanceSTCgraph.png "Security Test Cases!!")

This also helps in narrowing down the things that might not be working etc. based on some background check on plugin specifc errors.




## Post Scan Analysis (Hooks and customization) 

Here I will state a very common problem that come out of analysis phase, two very common issues these days with scanning are scanning REST APIs or Scanning Custom thick client(nw).
The problem with Rest api they are not consumed any where and it requires manual intervention because any auto spidering(profiling) or crawling will not unearth the API specs and exact behaviour.
So you need to work on it manually, but what if the number is big 100-500. That does make much sense right. So here is a little tweek I did to automatically fuzz any REST/SOAP APIs using the swagger json file only as input. This input can be directed to any DAST scanner and whole thing can be easily automated without compromising on results etc.

![ Swagger Automate Security Testing REST](/media/swaggerSecurityTestingAutomation-1.gif "swaggerSecurityTestingAutomation!!")







