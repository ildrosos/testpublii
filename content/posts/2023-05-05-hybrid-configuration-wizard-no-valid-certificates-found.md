---
title: Hybrid Configuration wizard - No Valid Certificates Found
slug: hybrid-configuration-wizard-no-valid-certificates-found
date: '2023-05-05T12:31:00+00:00'
status: published
excerpt: "Recently I had a case where a customer wanted to add a second exchange server in HCW to host receive and send connectors for redundancy .\_ The problem was that the server was in a different site and child domain than the 'primary' exchange server…"
featured_image: >-
  /images/posts/hybrid-configuration-wizard-no-valid-certificates-found/HCW-CertNotFound-3.png
categories: []
tags:
  - Hybrid Configuration Wizard
seo:
  meta_title: ''
  meta_description: "Recently I had a case where a customer wanted to add a second exchange server in HCW to host receive and send connectors for redundancy .\_ The problem was that the server was in a different site and child domain than the 'primary' exchange server…"
  og_image: >-
    /images/posts/hybrid-configuration-wizard-no-valid-certificates-found/HCW-CertNotFound-3.png
  noindex: false
---
Recently I had a case where a customer wanted to add a second exchange server in HCW to host receive and send connectors for redundancy . 

The problem was that the server was in a different site and child domain than the "primary" exchange server which was already part of HCW. 

As shown in the screenshot after selecting which servers you need to host Receive/Send connectors you need to select the Transport Certificate which will be used when connecting to Office 365. 

At this point I had the error : No valid certificates found for our 1st exchange server . 

After checking the server I identified that this was not true . The server had a valid cert and of course if you tried to run HCW from that server the error was gone. 

So what is the problem in this case ? The problem is that HCW is trying to query the server with the hostname as shown in the screenshot not the FQDN . But as mentioned the HCW was run in a different domain than the domain that the server in question was installed to . That said the hostname was not resolved to a real IP address .

So to overcome the problem you need to either  : 

Add a record to your local DNS server with the server name and the server IP or 

Add a hostfile  to your exchange server which will point to the real IP of the server we are looking for like this : 

<figure class="post__image post__image--center"><img alt="hostfile" height="305" src="/images/posts/hybrid-configuration-wizard-no-valid-certificates-found/ostfile.png" width="513"></figure>

After that if you did not close the HCW you will see this instead of the initial error : 

<figure class="post__image post__image--center"><img alt="HCW - waiting" height="590" src="/images/posts/hybrid-configuration-wizard-no-valid-certificates-found/HCW-WAITING.png" width="608"></figure>

I gave it about 30 mins with no result so I decided to make a coffee , and close and reopen HCW wizard after about 30 mins or so . This was the result : 

<figure class="post__image post__image--center"><img alt="" height="508" src="/images/posts/hybrid-configuration-wizard-no-valid-certificates-found/HCW-BOOOOM.png" width="566"></figure>

Boom , case closed. Cheers!
