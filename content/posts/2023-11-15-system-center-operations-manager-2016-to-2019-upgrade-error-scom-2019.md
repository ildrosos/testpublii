---
title: >-
  System Center Operations Manager 2016 to 2019 in-place Upgrade Error (SCOM
  2019)
slug: system-center-operations-manager-2016-to-2019-upgrade-error-scom-2019
date: '2023-11-15T12:58:00+00:00'
status: published
excerpt: >-
  While upgrading a SCOM 2016 instance to 2019 I came up to a strange error .
  While checking the prerequisites I verified that my 2016 SQL SP3 server was
  enough to proceed with the the installation . But while starting the
  installation I got an…
featured_image: >-
  /images/posts/system-center-operations-manager-2016-to-2019-upgrade-error-scom-2019/20162019featured-3.png
categories:
  - SCOM
tags:
  - SCOM
seo:
  meta_title: ''
  meta_description: >-
    While upgrading a SCOM 2016 instance to 2019 I came up to a strange error .
    While checking the prerequisites I verified that my 2016 SQL SP3 server was
    enough to proceed with the the installation . But while starting the
    installation I got an…
  og_image: >-
    /images/posts/system-center-operations-manager-2016-to-2019-upgrade-error-scom-2019/20162019featured-3.png
  noindex: false
---
While upgrading a SCOM 2016 instance to 2019 I came up to a strange error . While checking the prerequisites I verified that my 2016 SQL SP3 server was enough to proceed with the the installation . 

But while starting the installation I got an error in prerequisites check which stated that SQL version was not compatible with the current version I was trying to install . 

After finding the log folder of the installation I was running (C:\\Users\\userusedduringinstallation\\AppData\\Local\\SCOM\\LOGS) I opened SCOMPrereqCheck to check for more details . 

Unfortunately this log file has an XML like structure which does not provide more details to identify the root cause of this error . The only thing I found was a line stating OMDBSqlVersionCheckTitle with a Fail word in the end . 

Then I decided to check what was happening during the prerequisites check in my SQL server . So I fired Up SQL Management Studio logged in  and opened Management -> SQL Server Logs -> and opened the current logs file . 

There I saw a line saying : 

Starting up database 'SCOMINSTALLTESTDB\_638356414323093873'.

Each time I fired up the prerequisites check again on my SCOM server another database log was recorded . So to check if the server version is correct SCOM installer creates a test database with a random number in the end. I also checked that the account used during installation had the required permissions on the SQL server so that was not the issue. 

Then I decided to dig more . So I opened another log file through SQL Log viewer console which is called Windows NT . 

There I saw an error saying : 

The server-side authentication level policy does not allow the user DOMAIN\\USER SID (S-1-X-XX-XXXXXXXXXX-XXXXXXXXX-XXXXXXXXX-XXXXX) from address XX.XX.XX.XX to activate DCOM server. Please raise the activation authentication level at least to RPC\_C\_AUTHN\_LEVEL\_PKT\_INTEGRITY in client application.

I verified the IP listed here was the IP of my SCOM server . 

After googling the error I came up to this result : 

[https://support.microsoft.com/en-us/topic/kb5004442-manage-changes-for-windows-dcom-server-security-feature-bypass-cve-2021-26414-f1400b52-c141-43d2-941e-37ed901c769c](https://support.microsoft.com/en-us/topic/kb5004442-manage-changes-for-windows-dcom-server-security-feature-bypass-cve-2021-26414-f1400b52-c141-43d2-941e-37ed901c769c)

If you read the article you will end up installing the KB on your SQL server or adding the registry key specified with a value of 0 to bypass the error . I did the latter and restarted the SQL server since it is required for the reg change to work . 

Fired up prerequisites check again and the error was gone! Voila! 

P.S. That registry key imposes your server and infrastructure in a security risk . Please keep in mind to remove the registry key or find a workaround after installing your SCOM instance.
