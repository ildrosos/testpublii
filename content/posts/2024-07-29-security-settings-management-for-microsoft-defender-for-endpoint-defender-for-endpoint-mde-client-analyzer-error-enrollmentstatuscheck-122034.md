---
title: >-
  Security settings Management for Microsoft Defender for Endpoint - Defender
  for Endpoint MDE client analyzer - [Error] EnrollmentStatusCheck 122034
slug: >-
  security-settings-management-for-microsoft-defender-for-endpoint-defender-for-endpoint-mde-client-analyzer-error-enrollmentstatuscheck-122034
date: '2024-07-29T15:53:00+00:00'
status: published
excerpt: "In a recent case I was trying to deploy MDE policies like AV policies and ASR policies for devices not onboarded to Intune like my Windows Servers .\_ I noticed that in some cases the policies were showing as Pending in Intune report so I…"
featured_image: >-
  /images/posts/security-settings-management-for-microsoft-defender-for-endpoint-defender-for-endpoint-mde-client-analyzer-error-enrollmentstatuscheck-122034/Defender-for-endpoint-for-Servers3.jpeg
categories:
  - Defender
tags:
  - Defender for Endpoint for Servers
seo:
  meta_title: ''
  meta_description: "In a recent case I was trying to deploy MDE policies like AV policies and ASR policies for devices not onboarded to Intune like my Windows Servers .\_ I noticed that in some cases the policies were showing as Pending in Intune report so I…"
  og_image: >-
    /images/posts/security-settings-management-for-microsoft-defender-for-endpoint-defender-for-endpoint-mde-client-analyzer-error-enrollmentstatuscheck-122034/Defender-for-endpoint-for-Servers3.jpeg
  noindex: false
---
<figure class="post__image post__image--center"><img alt="MDEClient Analyzer" height="112" src="/images/posts/security-settings-management-for-microsoft-defender-for-endpoint-defender-for-endpoint-mde-client-analyzer-error-enrollmentstatuscheck-122034/DeviceConfigManagementDetails-3.png" width="591"></figure>

In a recent case I was trying to deploy MDE policies like AV policies and ASR policies for devices not onboarded to Intune like my Windows Servers . 

I noticed that in some cases the policies were showing as Pending in Intune report so I decided to run MDEClientAnalyzer tool to troubleshoot and find out what was wrong . 

The thing is that MDEClientAnalyzer showed an error which stated : 

> 122034 EnrollmentStatusCheck Policies assignment failure The device was successfully onboarded to Microsoft Defender for Endpoint and was able to download the endpoint security policies from MEM. However, there was a failure during the assignment of the policies.

I started checking the docs to find the error code 122034 or the description of the error but came out with nothing .

The problem was that my servers were Windows RDS hosts . If you check the official documentation you will find a statement which says :

> Security settings management doesn't work on and isn't supported with the following devices:  
>   
> Non-persistent desktops, like Virtual Desktop Infrastructure (VDI) clients  
> Azure Virtual Desktop (AVD and formerly Windows Virtual Desktop, WVD)  
> Domain Controllers  
> 32-bit versions of Windows

[https://learn.microsoft.com/en-us/mem/intune/protect/mde-security-integration](https://learn.microsoft.com/en-us/mem/intune/protect/mde-security-integration)

That said this is the error you get when your server falls in either of these categories .In my case Non-persistent desktops . 

So the error was not so self explanatory but now we know RDS servers MDE settings / policies cannot be configured through the new management feature called Security settings Management.
