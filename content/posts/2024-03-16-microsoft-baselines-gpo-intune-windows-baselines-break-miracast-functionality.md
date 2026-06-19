---
title: >-
  Microsoft Baselines (GPO) / Intune Windows Baselines break Miracast
  Functionality (connect to a wireless display).
slug: microsoft-baselines-gpo-intune-windows-baselines-break-miracast-functionality
date: '2024-03-16T00:57:00+00:00'
status: published
excerpt: >-
  Recently I run into a problem with one of my customers . After deploying the
  latest MIcrosoft Windows Baselines users were not able to use Connect to
  wireless diplay. I suspected that something in the baselines broke this
  functionality so I started searching within the…
featured_image: >-
  /images/posts/microsoft-baselines-gpo-intune-windows-baselines-break-miracast-functionality/Connect-to-wireless-display.jpg
categories: []
tags:
  - Microsoft Baselines
seo:
  meta_title: ''
  meta_description: >-
    Recently I run into a problem with one of my customers . After deploying the
    latest MIcrosoft Windows Baselines users were not able to use Connect to
    wireless diplay. I suspected that something in the baselines broke this
    functionality so I started searching within the…
  og_image: >-
    /images/posts/microsoft-baselines-gpo-intune-windows-baselines-break-miracast-functionality/Connect-to-wireless-display.jpg
  noindex: false
---
Recently I run into a problem with one of my customers . After deploying the latest MIcrosoft Windows Baselines users were not able to use Connect to wireless diplay.

I suspected that something in the baselines broke this functionality so I started searching within the settings.

The documentation provided did not mention miracast or cast at all so I decided to disable the settings , first the ones I thought that could lead to such a problem and later the others . 

Well if you are in the same situation search no more . 

After 2 hours of searching I found out that none of the settings I thought broke the functionality . The problem was Windows firewall. If you haven't enabled Windows firewall on your clients and apply the policies then windows firewall will be turned on .

But to find the exact process to exclude you need to know which that is and which firewall profile it is using . 

After all when you click on connect to a wireless display it turns out your computer is using wi-fi adapter to connect to a hotspot your television is broadcasting. That said the profile to change is the firewall Public profile since this is the network that is recognized as a public connection when your computer makes a direct connection with the TV/Screen to cast. 

So go and make an inbound exception on your firewall profile either from GPO/Intune/Manually for the app : 

-   Program %systemroot%/system32/WUDFHost.exe
-   Protocol: TCP

This should do the trick!
