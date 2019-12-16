---
title: "Silver Ticket in Cloud"
tags: ["Web Attack", "Security", "Hack", "Web Exploit", "Cloud Security"]
date: 2019-12-15
---



## Cloudiness in Web
With advent of new PAAS and FAAS servcies the scalability, development and deployment of web application has become easier as compared to traditional approach. Function as a service has been the talk of the town from quite some time. Deployment has never been easier for a web app/api. With FAAS and microservice concept the goto production time has been cut to minimum. Just like every other platform technology, FAAS security is the end customer/consumer's responsibility. Customer is responsible for putting a secure logic and has to take care of securing deployment secrets, account settings and code.
FAAS platform out of the box provides some secure/insecure features (security has to be configured :D).
In this article we will try to explore how a single vulnerable API can lead to compromise of entire application and cloud resources. Here is my attempt to getting SILVER ticket for application cloud resources.

## FAAS: Vulnerable API Case 

This is a simple case where REST API hosted on FAAS is running some logic to find health state of service/device.
![](/media/azFunc-1.png)

API seems to be working fine but with a few pickle-y inputs it started to behave strangely.

![](/media/azFunc-2.png)

After successfully exploiting the existing vulnerability, I was able to locate the application ID & Secret aka service principal.
Based on my basic understanding this is my silver ticket as this holds access to all resources within a service by design.

![](/media/wow.gif)


Let us use this to login to cloud management shell to find what all resources are owned by this particular service principal.

![](/media/azFunc-3.png)

Login to cloud shell was successful. Next, let us list down the resources.

![](/media/azFunc-4.png)


## My Silver Ticket 
So here goes my silver ticket for 36 resources.

![](/media/azFunc-5.png)






