---
title: >-
  Exchange Public Folders Migration - SourceSideValidation script shows
  OrphanedMPF folders.
slug: >-
  exchange-public-folders-migration-sourcesidevalidation-script-shows-orphanedmpf-folders
date: '2022-10-25T11:38:00+00:00'
status: published
excerpt: "So you are trying to run SourceSideVaildation.ps1 script provided from MS\_and find out OrphanedMPF objects that the script indicates that need to be cleaned up . The output of the script shows something like this :\_ As granikos explains in more detail : https://granikos.eu/exchange-public-folder-meso-object-not-deleted/ So…"
featured_image: >-
  /images/posts/exchange-public-folders-migration-sourcesidevalidation-script-shows-orphanedmpf-folders/microsoft-exchange-logo_resized.png
categories:
  - MS Exchange
tags:
  - Exchange
seo:
  meta_title: ''
  meta_description: "So you are trying to run SourceSideVaildation.ps1 script provided from MS\_and find out OrphanedMPF objects that the script indicates that need to be cleaned up . The output of the script shows something like this :\_ As granikos explains in more detail : https://granikos.eu/exchange-public-folder-meso-object-not-deleted/ So…"
  og_image: >-
    /images/posts/exchange-public-folders-migration-sourcesidevalidation-script-shows-orphanedmpf-folders/microsoft-exchange-logo_resized.png
  noindex: false
---
So you are trying to run SourceSideVaildation.ps1 script provided from [MS](https://techcommunity.microsoft.com/t5/exchange-team-blog/making-your-public-folder-migrations-faster-and-more-reliable/ba-p/917622) and find out OrphanedMPF objects that the script indicates that need to be cleaned up . The output of the script shows something like this : 

<figure class="post__image post__image--center"><img alt="OrphanedMPFs" height="36" src="/images/posts/exchange-public-folders-migration-sourcesidevalidation-script-shows-orphanedmpf-folders/OrphanedMPFs.png" width="744"></figure>

As granikos explains in more detail : [https://granikos.eu/exchange-public-folder-meso-object-not-deleted/](<https://As granikos explains in more detail : https://granikos.eu/exchange-public-folder-meso-object-not-deleted/>)

So the next thing to do is to find the orphaned entries and delete them . 

So after searching ADSI in the appropriate path I found 2 entries as expected : 

<figure class="post__image post__image--center"><img alt="" height="448" src="/images/posts/exchange-public-folders-migration-sourcesidevalidation-script-shows-orphanedmpf-folders/PublicFoldersADSI.png" width="1027"></figure>

  

The right one to delete is the one without random numbers after it's name but to be sure check the when created date. As you noticed the one with the random numbers was indeed created after the one without numbers. This confirms that the one without random numbers is the right one that needs to be deleted. 

In my case after re-running the script the folder with the number re-appeared in suggestions . This confirms that the folder is no longer mail-enabled and the entries have to be deleted (both) . But to be sure check if the public folder is mail-enabled before deleting both entries.
