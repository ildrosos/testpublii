---
title: >-
  Email messages sent through Outlook have a padlock icon next to the message
  but message was not sent encrypted.
slug: >-
  email-messages-sent-through-outlook-have-a-padlock-icon-next-to-the-message-but-message-was-not-sent-encrypted
date: '2022-07-12T16:38:00+00:00'
status: published
excerpt: >-
  Sometimes you choose to send a message through outlook with a digital
  signature but without enryption. If you check your sent messages there is a
  padlock icon next to each message but you've sent the message without
  encryption : This is happening because the following…
featured_image: >-
  /images/posts/email-messages-sent-through-outlook-have-a-padlock-icon-next-to-the-message-but-message-was-not-sent-encrypted/outlook-padlock-res.png
categories:
  - M365 Migration
tags:
  - M365 Apps
seo:
  meta_title: ''
  meta_description: >-
    Sometimes you choose to send a message through outlook with a digital
    signature but without enryption. If you check your sent messages there is a
    padlock icon next to each message but you've sent the message without
    encryption : This is happening because the following…
  og_image: >-
    /images/posts/email-messages-sent-through-outlook-have-a-padlock-icon-next-to-the-message-but-message-was-not-sent-encrypted/outlook-padlock-res.png
  noindex: false
---
Sometimes you choose to send a message through outlook with a digital signature but without enryption. 

If you check your sent messages there is a padlock icon next to each message but you've sent the message without encryption : 

<figure class="post__image"><img alt="outlook-padlock" height="455" src="/images/posts/email-messages-sent-through-outlook-have-a-padlock-icon-next-to-the-message-but-message-was-not-sent-encrypted/outlook-padlock.png" width="1033"></figure>

This is happening because the following option is not checked : 

<figure class="post__image"><img alt="TrustCenterSettings" height="669" src="/images/posts/email-messages-sent-through-outlook-have-a-padlock-icon-next-to-the-message-but-message-was-not-sent-encrypted/smime-check.png" width="823"></figure>

To resolve the issue navigate to : 

Outlook

File - Options - Trust center - Trust Center Settings - Email Security and activate 

Send clear text signed message when sending signed messages.
