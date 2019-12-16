---
title: "Silver Ticket in Cloud"
tags: ["Web Attack", "Security", "Hack", "Web Exploit", "Cloud Security"]
date: 2019-12-15
---



## Cloudiness in Web
With advent of new PAAS and FAAS servcies the scalability, development and deployment of web application has been made easier as compared with traditional approach. Function as a service has been the talk of the town from quite some time. Deployment has never been so easy for a web app/api but with FAAS and microservice concept the goto production time has been cut to minimum. Though like with every technology in FAAS also, it is also the end customer/consumer responsibility to put a secure logic and take responsibility of securing deployment secrets, account settings and code.
As any new platform out of the box provide some secure/insecure features (security one requires configure :D).
In this article we will try to explore how in absence of these security settings and logic may lead to compromise of entire application resources from a single vulnerable FAAS API. Also, try to get SILVER ticket for application/Resource group.

## FAAS: Vulnerable API Case 

This is a simple case where REST API hosted on FAAS, running some logic to find health state of service/device.
![](/media/azFunc-1.png)

API seems to be working fine but with few pickle-y inputs it started to behave strangely.

![](/media/azFunc-2.png)

After successfully exploiting the existing vulnerability, I was able to locate the application ID & Secret aka service principal.
Based on my basic understanding this is my silver ticket as this holds access to all resources within a service by design.

![](/media/wow.gif)


Let use this to login to cloud management shell to find what all resource is owned by this particular service principal.

![](/media/azFunc-3.png)

Login successful to cloud shell. Next, lets list down the resources.

![](/media/azFunc-4.png)


## My Silver Ticket 
So here goes my silver ticket for 36 resources.

![](/media/azFunc-5.png)






