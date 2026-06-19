---
title: >-
  Move CodeTwo Office 365 Migration tool to another computer (not an exchange
  server)
slug: >-
  move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server
date: '2022-07-12T16:39:00+00:00'
status: published
excerpt: >-
  If you need to move CodeTwo Office 365 Migration tool to another computer but
  do not want to purge mailboxes which were already processed and start over (to
  avoid duplicates creation) you need to follow the workaround mentioned in the
  document below to migrate settings…
featured_image: >-
  /images/posts/move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server/MoveCodeTwoToAnothercomputer.png
categories:
  - M365 Migration
tags:
  - CodeTwoOffice365Migration
seo:
  meta_title: ''
  meta_description: >-
    If you need to move CodeTwo Office 365 Migration tool to another computer
    but do not want to purge mailboxes which were already processed and start
    over (to avoid duplicates creation) you need to follow the workaround
    mentioned in the document below to migrate settings…
  og_image: >-
    /images/posts/move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server/MoveCodeTwoToAnothercomputer.png
  noindex: false
---
If you need to move CodeTwo Office 365 Migration tool to another computer but do not want to purge mailboxes which were already processed and start over (to avoid duplicates creation) you need to follow the workaround mentioned in the document below to migrate settings and cache objects of the software to the new computer: [CodeTwo Documentation](https://www.codetwo.com/userguide/exchange-rules-pro/reinstall-or-move.htm)

What CodeTwo does not mention in the article is that you need to have Messaging Api And Collaboration Data Objects 1.2.1 installed on the computer in case you are trying to migrate from Exchange 2003 or 2007. 

If you already have office installed on your computer you need to remove it first before installing CodeTwo as per the guide's instructions. If you don't **like I did** Messaging Api won't be installed automatically and you won't be able to start the jobs or reconfigure the source server connection. 

<figure class="post__image"><img alt="MessagingAndCollaborationApi" height="640" src="/images/posts/move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server/MessagingAndCollaborationAPI-6.png" width="1007"></figure>

You can always remove and reinstall after removing Office from the "host" computer but you can also navigate to Add/Remove Programs and repair the installation of CodeTwo . If you do so a message will popup which indicates that Messaging Api And Collaboration Data Objects 1.2.1 is not installed on the computer and ask if you want the software to be installed. If you do so source server connection will be established .

When you try to restart your jobs you will get another error indicating that a certificate with a particular thumbprint is missing from the computer so the jobs will fail to start. 

<figure class="post__image"><img alt="CodeTwoMissingCertError" height="168" src="/images/posts/move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server/CodeTwoMissingCertError.png" width="385"></figure>

You can then export the required certificate and import it to your "host" computer under User/Personal store. 

<figure class="post__image"><img alt="ExportCodeTwoCertFromSourceServer" height="622" src="/images/posts/move-codetwo-office-365-migration-tool-to-another-computer-not-an-exchange-server/ExportCodeTwoCertFromSourceServer.png" width="1034"></figure>

After that all migrations will start like you were in your old machine and mail duplicates won't exist. No need to purge your mailboxes either this way.
