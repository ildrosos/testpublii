---
title: >-
  Windows Hello for Business Pin Reset Client / Service Production Applications
  are not working as expected - PIN reset is not working on Hybrid Joined
  devices.
slug: >-
  windows-hello-for-business-pin-reset-client-service-production-applications-are-not-working-as-expected
date: '2024-07-02T15:30:00+00:00'
status: published
excerpt: >-
  If you are trying to deploy the applications published from Microsoft to
  enable Pin Reset from lockscreen on your Entra ID Joined / Entra Hybrid Joined
  devices you are probably reading this article :
  https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/pin-reset?tabs=intune
  Following the guide to enable the PIN reset you need…
featured_image: >-
  /images/posts/windows-hello-for-business-pin-reset-client-service-production-applications-are-not-working-as-expected/Win11LockScreenPINReset1.jpeg
categories:
  - Intune
tags:
  - Microsoft Intune
seo:
  meta_title: ''
  meta_description: >-
    If you are trying to deploy the applications published from Microsoft to
    enable Pin Reset from lockscreen on your Entra ID Joined / Entra Hybrid
    Joined devices you are probably reading this article :
    https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/pin-reset?tabs=intune
    Following the guide to enable the PIN reset you need…
  og_image: >-
    /images/posts/windows-hello-for-business-pin-reset-client-service-production-applications-are-not-working-as-expected/Win11LockScreenPINReset1.jpeg
  noindex: false
---
If you are trying to deploy the applications published from Microsoft to enable Pin Reset from lockscreen on your Entra ID Joined / Entra Hybrid Joined devices you are probably reading this article : 

[https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/pin-reset?tabs=intune](https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/pin-reset?tabs=intune)

Following the guide to enable the PIN reset you need to register two applications :

 [Microsoft PIN Reset Service Production website](https://login.windows.net/common/oauth2/authorize?response_type=code&client_id=b8456c59-1230-44c7-a4a2-99b085333e84&resource=https%3A%2F%2Fgraph.windows.net&redirect_uri=https%3A%2F%2Fcred.microsoft.com&state=e9191523-6c2f-4f1d-a4f9-c36f26f89df0&prompt=admin_consent)

and

[Microsoft PIN Reset Client Production website](https://login.windows.net/common/oauth2/authorize?response_type=code&client_id=9115dd05-fad5-4f9c-acc7-305d08b1b04e&resource=https%3A%2F%2Fcred.microsoft.com%2F&redirect_uri=ms-appx-web%3A%2F%2FMicrosoft.AAD.BrokerPlugin%2F9115dd05-fad5-4f9c-acc7-305d08b1b04e&state=6765f8c5-f4a7-4029-b667-46a6776ad611&prompt=admin_consent)

After logging in you see the prompts published in the article but at the next step you get an error about the Reply URL for the Service Production Website and a "white" screen for the Client Production website . 

That doesn't mean the apps are not deployed but they are not deployed as they should .

Some information and permissions are missing . 

**So next step will be to go the apps and Grant admin permissions . Even you don't see any permissions underneath Grant admin consent and wait 2-5 minutes . After 5 minutes approximately permissions will appear and the apps will be ready to be used.**

For reference here are the IDs of the correct apps :  Microsoft Pin Reset Client Production application 

9115dd05-fad5-4f9c-acc7-305d08b1b04e

Microsoft Pin Reset Service  Production Application

b8456c59-1230-44c7-a4a2-99b085333e84

This is the result , notice that only one of them has a Homepage URL : 

<figure class="post__image"><img alt="Pin reset snip" height="94" src="/images/posts/windows-hello-for-business-pin-reset-client-service-production-applications-are-not-working-as-expected/Microsoft-PIN-reset.png" width="1623"></figure>

Now retry resetting the PIN from the lockscreen on your devices .

Don't forget to exclude these two enterprise Apps from any conditional access policies which enforce MFA (if you have policies with all apps selected exclude only these two) as they seem to interfere with the process and they are totally unnecessary since MFA is enforced either way .

In my tests on Windows 10 it works perfectly fine but on Windows 11 the button Forgot My PIN does nothing . I read this is a known behavior and to workaround it you need to follow these steps : 

-   Chose "Other User"
-   Switch to PIN code 
-   Enter the username of the user (without @domain.com)
-   Finally
-   Click on PIN Reset  
      
    

This will guide you to start the PIN reset process by entering your password first etc.
