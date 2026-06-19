---
title: "How can I install\_Microsoft 365 Apps via Intune from local source."
slug: how-can-i-install-microsoft-365-apps-via-intune-from-local-source
date: '2022-10-25T15:10:00+00:00'
status: published
excerpt: "Did you know you can initiate Microsoft 365 Apps via Intune when the source files are in your file server? Nor did I.\_ Usecase : Customer needs to be able to push Microsoft 365 Apps via intune but network bandwidth is a huge stopper when…"
featured_image: >-
  /images/posts/how-can-i-install-microsoft-365-apps-via-intune-from-local-source/MicrosoftIntuneM365AppsLocalSource.png
categories:
  - Intune
tags:
  - Microsoft Intune
seo:
  meta_title: ''
  meta_description: "Did you know you can initiate Microsoft 365 Apps via Intune when the source files are in your file server? Nor did I.\_ Usecase : Customer needs to be able to push Microsoft 365 Apps via intune but network bandwidth is a huge stopper when…"
  og_image: >-
    /images/posts/how-can-i-install-microsoft-365-apps-via-intune-from-local-source/MicrosoftIntuneM365AppsLocalSource.png
  noindex: false
---
Did you know you can initiate Microsoft 365 Apps via Intune when the source files are in your file server? Nor did I. 

Usecase : Customer needs to be able to push Microsoft 365 Apps via intune but network bandwidth is a huge stopper when multiple (thousands) Windows Devices try to reach Office CDN and download the required setup files. 

So you allow all Intune Management Urls in order to manage endpoints via Intune but do not want to allow Office CDN Urls access to control bandwidth within the organization . 

Well there is a way to do that. 

There is only 1 Requirement . Your file server or any server which will download the required setup files needs to have proper Internet access only once. When the server downloads the setup files. Internet Access can be prohibited afterwards. 

So go ahead and download Office Deployment tool : [https://learn.microsoft.com/en-us/deployoffice/overview-office-deployment-tool](https://learn.microsoft.com/en-us/deployoffice/overview-office-deployment-tool)

Next go to Office Customization Tool : [https://config.office.com/deploymentsettings](https://config.office.com/deploymentsettings)

Choose which apps you need to be installed on the Windows Clients (managed by Intune) and of course the rest of the settings like languages  ,activation etc. 

Do not forget to add the local source the office files will reside on for example: 

<figure class="post__image post__image--center"><img alt="" height="289" src="/images/posts/how-can-i-install-microsoft-365-apps-via-intune-from-local-source/IntuneOfficeInstallationViaLocalSource.png" width="857"></figure>

Export the configuration xml let's say it's called configuration.xml.

Now login to the server which will host the setup files . 

Move configuration.xml and Office Deployment tool to the server. Create a shared folder with proper permissions so your windows devices can reach and read the setup files. 

Run setup.exe with the configuration.xml and the download switch for example : 

setup.exe /download configuration.xml

Wait for the download process to complete . While waiting copy the content of configuration.xml to notepad . 

Go ahead and create a new app within intune of type Microsoft 365 Apps :

<figure class="post__image post__image--center"><img alt="" height="250" src="/images/posts/how-can-i-install-microsoft-365-apps-via-intune-from-local-source/Microsoft365-Apps.png" width="568"></figure>

At the configure app suite step choose Enter XML Data 

<figure class="post__image post__image--center"><img alt="" height="662" src="/images/posts/how-can-i-install-microsoft-365-apps-via-intune-from-local-source/ConfigurationDesignerXML.png" width="843"></figure>

Paste the configuration.xml data you have from notepad inside the XML Data and validate the XML . When XML is validated go ahead and assign the  app . 

Done!
