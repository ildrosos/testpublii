---
title: Exchange Server Databases "Dissappeared"
slug: exchange-server-databases-dissappeared
date: '2022-07-04T11:12:00+00:00'
status: published
excerpt: Exchange server databases dissapeared!
featured_image: >-
  /images/posts/exchange-server-databases-dissappeared/microsoft-exchange-logo_resized.png
categories:
  - MS Exchange
tags:
  - Exchange
seo:
  meta_title: ''
  meta_description: Exchange server databases dissapeared!
  og_image: >-
    /images/posts/exchange-server-databases-dissappeared/microsoft-exchange-logo_resized.png
  noindex: false
---
After logging in to exchange on premise (ECP) you might see the picture below when navigating to Servers - > Databases

<figure class="post__image"><img alt="" height="483" src="/images/posts/exchange-server-databases-dissappeared/exc.jpg" width="881"></figure>

In addition if you check through Exchange management shell you might get the error below or similar like we did : 

<figure class="post__image"><img alt="" height="161" src="/images/posts/exchange-server-databases-dissappeared/excdb-2.jpg" width="950"></figure>

So it seems there is an old reference to a database which has been deleted in our case . But why should this interfere with the rest of our Databases?

We deleted the reference to the old database - Make sure it is an old reference which is no longer needed before deletion - by navigating to ADSI Edit : 

On a Domain Controller, Open ADSIEdit.msc and Connect to ‘Configuration’.

Navigate to Configuration > Services > Microsoft Exchange > {Organisation name} > Administrative Groups > {Administrative-Group-Name} > Databases  >Delete the old database reference . 

<figure class="post__image"><img alt="" height="327" src="/images/posts/exchange-server-databases-dissappeared/excADSI.jpg" width="665">&nbsp;</figure>

After 5 minutes logout and relogin to ECP and check again . Your databases should be visible now :D .
