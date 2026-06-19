---
title: "IMAP migration -\_ Yahoo (free) accounts to Microsoft 365 fails!"
slug: imap-migration-yahoo-free-accounts-to-microsoft-365
date: '2024-07-09T17:08:00+00:00'
status: published
excerpt: "Are you trying to migrate IMAP Yahoo accounts to 365 and get errors ? Like incorrect username or password ? Follow these steps :\_ If you are trying to create the migration endpoint for IMAP migrations to your Microsoft 365 tenant remember to use the…"
featured_image: >-
  /images/posts/imap-migration-yahoo-free-accounts-to-microsoft-365/YahoomailIMAPMigrationFailed.jpeg
categories:
  - M365 Migration
tags:
  - M365Migration
seo:
  meta_title: ''
  meta_description: "Are you trying to migrate IMAP Yahoo accounts to 365 and get errors ? Like incorrect username or password ? Follow these steps :\_ If you are trying to create the migration endpoint for IMAP migrations to your Microsoft 365 tenant remember to use the…"
  og_image: >-
    /images/posts/imap-migration-yahoo-free-accounts-to-microsoft-365/YahoomailIMAPMigrationFailed.jpeg
  noindex: false
---
Are you trying to migrate IMAP Yahoo accounts to 365 and get errors ? Like incorrect username or password ? Follow these steps : 

If you are trying to create the migration endpoint for IMAP migrations to your Microsoft 365 tenant remember to use the settings below : 

<figure class="post__image post__image--center"><img alt="MigrationEndpoint Settings IMAP server" height="521" src="/images/posts/imap-migration-yahoo-free-accounts-to-microsoft-365/MigrateImapEndpointSettings.png" width="307"></figure>

-   Next you will need to create your 365 users
-   Assign a license to the users
-   And finally prepare the CSV file  

About the CSV file be careful with the columns : 

Email address: column is your destination 365 mailbox email address.

Username: is your Yahoo account username without @ sign or anything else.

Password:Is your account's app password  .

<figure class="post__image post__image--center"><img alt="CSV details" height="45" src="/images/posts/imap-migration-yahoo-free-accounts-to-microsoft-365/CSV.png" width="664"></figure>

If you try to use your password instead you will get multiple errors . Go ahead and create an app password on this link  : [AppPasswordsCreation](https://login.yahoo.com/myaccount/security/app-password/?intl=us&lang=el-GR&src=member-center&activity=ybar-acctOverview&pspid=794264123&theme=light&done=https%3A%2F%2Flogin.yahoo.com%2Fmyaccount%2Foverview%2F%3Fsrc%3Dym%26.done%3Dhttps%253A%252F%252Fmail.yahoo.com%252F%253F.intl%253Dgr%2526.lang%253Del-GR%26pspid%3D159628501%26activity%3Dybar-acctinfo%26.scrumb%3DJIurewD6tGX&scrumb=e1Y.HyJAjDK&backTo=%2Fsecurity%2F%3Fintl%3Dus%26lang%3Del-GR%26src%3Dmember-center%26activity%3Dybar-acctOverview%26pspid%3D794264123%26theme%3Dlight%26done%3Dhttps%253A%252F%252Flogin.yahoo.com%252Fmyaccount%252Foverview%252F%253Fsrc%253Dym%2526.done%253Dhttps%25253A%25252F%25252Fmail.yahoo.com%25252F%25253F.intl%25253Dgr%252526.lang%25253Del-GR%2526pspid%253D159628501%2526activity%253Dybar-acctinfo%2526.scrumb%253DJIurewD6tGX%26scrumb%3De1Y.HyJAjDK%26anchorId%3DappPasswordCard)

After creating the app password write it down in your CSV file in Password column and Start your migration batch.
