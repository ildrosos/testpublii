---
title: "ADConnect Error -\_DeletingLicensedGroupNotAllowed"
slug: adconnect-error-deletinglicensedgroupnotallowed
date: '2023-01-13T09:38:00+00:00'
status: published
excerpt: "If you ever delete a group in Azure AD which is used for Group Licensing purposes you will receive an error in ADConnect similar to the screenshot above .\_ The error states : 'DeletingLicensedGroupNotAllowed' The problem is that AD Connect does not specifically tell you…"
featured_image: >-
  /images/posts/adconnect-error-deletinglicensedgroupnotallowed/DeletingLicensedGroupNotAllowedADConnect-2.png
categories: []
tags:
  - ADConnect
seo:
  meta_title: ''
  meta_description: "If you ever delete a group in Azure AD which is used for Group Licensing purposes you will receive an error in ADConnect similar to the screenshot above .\_ The error states : 'DeletingLicensedGroupNotAllowed' The problem is that AD Connect does not specifically tell you…"
  og_image: >-
    /images/posts/adconnect-error-deletinglicensedgroupnotallowed/DeletingLicensedGroupNotAllowedADConnect-2.png
  noindex: false
---
<figure class="post__image post__image--center"><img alt="" height="517" src="/images/posts/adconnect-error-deletinglicensedgroupnotallowed/DeletingLicensedGroupNotAllowedADConnect.png" width="1025"><figcaption>DeletingLicensedGroupNotAllowed</figcaption></figure>

If you ever delete a group in Azure AD which is used for Group Licensing purposes you will receive an **error** in **ADConnect** similar to the screenshot above . 

The error states : "**DeletingLicensedGroupNotAllowed**"

The problem is that AD Connect does not specifically tell you which AD Group caused the problem. In most situations you realise you have an error in AD Connect a few hours or days later . 

What if it was Azure Active Directory clean up day and you deleted 200 groups ? How can I find which group is causing the problem ?  

Navigate to **Azure Active Directory** - > **Groups** and then **Audit Logs** . 

Click add **filters** and add **Status** . Then choose **failure** for **status** . Obviously within the failures you will find the one you are looking for . To resolve the error **remove the license assignment from the group** so the group can be deleted.
