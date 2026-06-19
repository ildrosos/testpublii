---
title: >-
  Manage Exhange Online Mailbox Quotas based on group membership through
  Powershell
slug: >-
  manage-exhange-online-mailbox-quotas-based-on-group-membership-through-powershell
date: '2022-07-13T16:35:00+00:00'
status: published
excerpt: >-
  If you need to change Office365 mailbox quotas the only way to do so is via
  Powershell. In addition there is not way to control quotas via groups for
  example or any policy to assign in order to assisgn for example X GBs quota
  to…
featured_image: >-
  /images/posts/manage-exhange-online-mailbox-quotas-based-on-group-membership-through-powershell/mailboxquotas.png
categories:
  - MS Exchange
tags:
  - Exchange
seo:
  meta_title: ''
  meta_description: >-
    If you need to change Office365 mailbox quotas the only way to do so is via
    Powershell. In addition there is not way to control quotas via groups for
    example or any policy to assign in order to assisgn for example X GBs quota
    to…
  og_image: >-
    /images/posts/manage-exhange-online-mailbox-quotas-based-on-group-membership-through-powershell/mailboxquotas.png
  noindex: false
---
If you need to change Office365 mailbox quotas the only way to do so is via Powershell. In addition there is not way to control quotas via groups for example or any policy to assign in order to assisgn for example X GBs quota to sales department . 

For that reason I created the Powershell script below . 

Feel free to change / share / comment .

Script contains parts of the script originally posted by 

Johan Dahlbom from 365lab.net so credits to Johan :D 

Script Logic : 

-   Checks if the script was run under admin privileges otherwise informs the user to rerun with admin privs.
-   Checks if MSOnline and Exchange Online Modules are installed . 
-   If they are not it tries to install them . 
-   Checks if there are active MSOnline and Exchange Online Sessions. If there are not it prompts you to enter credentials to connect .
-   Asks the user to provide the name of the group which will get the following quota policy (of course you can change the values to suit your needs) : 
    
    ProhibitSendQuota 4.5gb -ProhibitSendReceiveQuota 5gb -IssueWarningQuota 3.9gb
    
    Asks  the user to provide the script output folder (usefull when you run it against multiple users) . 
    
-   Then it changes all group's members quota to the one mentioned above
-   And also changes all other users quotas to 
    
    ProhibitSendQuota 75gb -ProhibitSendReceiveQuota 80gb -IssueWarningQuota 74gb
    

The Script : 

`function EditMailboxQuotas {`

    `$OutPutFolder = "c:\temp\EditMailboxQuotas.txt"`

    `$SpecificUserQuota=$null`

    `$ConvertedQuotaToKB = $null`

    `Write-Host "Please input the name of the group members of which will be assigned 5GB Quota"`

        `$ADGroup = Read-Host -Prompt 'Input the group name'`

        `Write-Host "Input the folder to export all script output - Please include file name as well" -ForegroundColor Green`

        `$OutPutFolder = Read-Host -Prompt 'Folder path and file name to export output to "Example:c:\filename.txt"'`

        `Write-Host "Searching for Group " $ADGroup`

        `Write-Output "Searching for Group " $ADGroup | Out-File $OutPutFolder -Append`

        `Import-Module AzureAD`

        `Import-Module MSOnline`

        `Import-Module ExchangeOnlineManagement`

        `#Connect-MsolService`

        `$MSOLAccountSku = Get-MsolAccountSku -ErrorAction Ignore -WarningAction Ignore`

            `if (-not($MSOLAccountSku)) {`

                `Connect-MsolService`

            `}`

        `$getsessions = Get-PSSession | Select-Object -Property State, Name`

        `#Connect-ExchangeOnline`

        `$IsconnectedToEOL = (@($getsessions) -like '@{State=Opened; Name=ExchangeOnlineInternalSession*').Count -gt 0`

            `If ($IsconnectedToEOL -ne "True") {`

            `Connect-ExchangeOnline`

            `}`  

        `#Connect-ExchangeOnline`

        `$MSOLGroup = Get-MsolGroup -SearchString $ADGroup`

        `Write-Host "Group's id is  " $MSOLGroup.ObjectId`

        `Write-Output "Group's id is  " $MSOLGroup.ObjectId | Out-File $OutPutFolder -Append`

        `write-host "------------- Configuring quotas for all users who are members of the group  " $ADGroup "-------------" -ForegroundColor Magenta`

        `Write-Output "------------- Configuring quotas for all users who are members of the group  " $ADGroup "-------------" | Out-File $OutPutFolder -Append`

        `Read-Host -Prompt "Press any key to continue configuring quotas for all users who are members of the group to 5GB or CTRL+C to quit"`

        `Start-Sleep -Seconds 3`

        `$UsersWithinGroup = Get-JDMsolGroupMember($MSOLGroup.ObjectId)`

        `if($null -ne $UsersWithinGroup)`

            `{#Check if mailbox quota more than 5gb`

            `foreach ($UserWithinGroup in $UsersWithinGroup)`

             `{     $UserWithinGroup.EmailAddress`

                   `$SpecificUserQuota = Get-Mailbox $UserWithinGroup.EmailAddress | Select-Object ProhibitSendReceiveQuota`

                   `$ConvertedQuotaToKB = Convert-QuotaStringToKB($SpecificUserQuota)`

             `If($ConvertedQuotaToKB -gt 5242880 ) {#quota is more than 5gb`

                `Write-Host "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota more than 5gb will check mailbox size" -ForegroundColor Cyan`

                `Write-Output "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota more than 5gb will check mailbox size" | Out-File $OutPutFolder -Append`

                `##Check if mailbox size is more than 4.5`

                `$MailboxSize =  Get-MailboxStatistics $UserWithinGroup.EmailAddress | Select-Object TotalItemSize`

                `$MailboxSizeInKB = Convert-QuotaStringToKB($MailboxSize)`

                    `if ($MailboxSizeInKB -gt 4718592){#If user mailbox size is more than 4.5 GB`

                        `Write-Host "Mailbox size is more than 4.5 GB No changes will be made " -ForegroundColor Red`

                        `Write-Output "Mailbox size is more than 4.5 GB No changes will be made " -Append`

                        `#do Nothing`

                            `}    #If user mailbox size is more than 4.5 GB`

                        `if ($MailboxSizeInKB -lt 4718592) {#Mailbox size is less than 4.5 GB`

                        `Write-Host "CurrentUser" $UserWithinGroup.EmailAddress "has" $MailboxSizeInKB "KB mailbox size mailbox quota will be changed" -ForegroundColor Green`

                        `Write-Output "CurrentUser" $UserWithinGroup.EmailAddress "has" $MailboxSizeInKB "KB mailbox size mailbox quota will be changed" | Out-File $OutPutFolder -Append`

                        `set-Mailbox $UserWithinGroup.EmailAddress -ProhibitSendQuota 4.5gb -ProhibitSendReceiveQuota 5gb -IssueWarningQuota 3.9gb`

                        `$SpecificUserQuota = $null`

                            `}   #Mailbox size is less than 4.5 GB`

                        `}`              

                        `elseif ($ConvertedQuotaToKB -eq 5242880 ) { #quota is 5gb`

                            `##Do nothing`

                         `Write-Host "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota 5gb Nothing will be changed"  -ForegroundColor Red`

                         `Write-Output "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota 5gb Nothing will be changed" | Out-File $OutPutFolder -Append`

                         `$SpecificUserQuota = $null`

                        `}`

                        `elseIf ($UserWithinGroup.UseDatabaseQuotaDefaults) #quota used from db defaults`

                        `{   Write-Host "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "uses defaults changing"  -ForegroundColor Red`

                        `Write-Output "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "uses defaults changing" | Out-File $OutPutFolder -Append`

                        `set-Mailbox $UserWithinGroup.EmailAddress -ProhibitSendQuota 4.5gb -ProhibitSendReceiveQuota 5gb -IssueWarningQuota 3.9gb`

                        `}`

                        `elseif ($ConvertedQuotaToKB -lt 5242880) #quota is less than 5gb and not db defaults`

                        `{   Write-Host "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota less than 5gb Quotas will be changed"  -ForegroundColor Green`

                        `Write-Output "CurrentUser" $UserWithinGroup.EmailAddress "has" $ConvertedQuotaToKB "KB Quota less than 5gb Quotas will be changed" | Out-File $OutPutFolder -Append`

                        `set-Mailbox $UserWithinGroup.EmailAddress -ProhibitSendQuota 4.5gb -ProhibitSendReceiveQuota 5gb -IssueWarningQuota 3.9gb`

                        `}`

        `}#ForEach`

    `}#If`

  

        `else {`

            `#GroupIsEmpty`

        `}`

  

        `write-host "------------- Configuring quotas for all users EXCLUDING those who are members of the group " $ADGroup "-------------" -ForegroundColor Magenta`

        `Write-Output write-host "------------- Configuring quotas for all users EXCLUDING those who are members of the group " $ADGroup "-------------" | Out-File $OutPutFolder -Append`

        `Read-Host -Prompt "Press any key to continue configuring quotas for ALL usersof the organization who are not members of the group to 80GB or CTRL+C to quit"`

        `Start-Sleep -Seconds 3`

        `$UsersWithinGroup = Get-JDMsolGroupMember($MSOLGroup.ObjectId)`  

        `$EXOMailboxes = Get-EXOMailbox -RecipientTypeDetails UserMailbox`

    `foreach($EXOMailbox in $EXOMailboxes)`

        `{write-host "For mailbox  "$EXOMailbox.primarySMtpaddress -ForegroundColor White`

        `Write-Output "For mailbox  "$EXOMailbox.primarySMtpaddress | Out-File $OutPutFolder -Append`

            `if(!(Get-MsolGroupMember -GroupObjectId $MSOLGroup.ObjectId | Where-Object {$_.EmailAddress -eq $EXOMailbox.primarySMtpaddress}))`

                `{write-host "Configuring Quota For mailbox  "$EXOMailbox.primarySMtpaddress " to 80GB" -ForegroundColor Green`

                `Write-Output "Configuring Quota For mailbox  "$EXOMailbox.primarySMtpaddress " to 80GB" | Out-File $OutPutFolder -Append`

                `set-Mailbox $EXOMailbox.primarySMtpaddress -ProhibitSendQuota 75gb -ProhibitSendReceiveQuota 80gb -IssueWarningQuota 74gb`

                `}#if`

                `elseif (Get-MsolGroupMember -GroupObjectId $MSOLGroup.ObjectId | Where-Object {$_.EmailAddress -eq $EXOMailbox.primarySMtpaddress})`

                `{`  

                    `write-host "For mailbox  "$EXOMailbox.primarySMtpaddress "Nothing will be changed because user is a member of the group" $ADGroup -ForegroundColor red`  

                    `Write-Output "For mailbox  "$EXOMailbox.primarySMtpaddress "Nothing will be changed because user is a member of the group" $ADGroup | Out-File $OutPutFolder -Append`

                `}`

        `}#ForEachEXOMailbox`

        `Write-Host "All quotas configuration is finished . If you need to change quotas again re-run the script."  -ForegroundColor Gray`

        `Write-Output  "All quotas configuration is finished . If you need to change quotas again re-run the script." | Out-File $OutPutFolder -Append`

`}#FunctionEditMailboxQuotas`  

`Function Convert-QuotaStringToKB() {`

  

    `Param([string]$CurrentQuota)`

  

    `[string]$CurrentQuota = ($CurrentQuota.Split("("))[1]`

    `[string]$CurrentQuota = ($CurrentQuota.Split(" bytes)"))[0]`

    `$CurrentQuota = $CurrentQuota.Replace(",","")`

    `[int]$CurrentQuotaInKB = "{0:F0}" -f ($CurrentQuota/1024)`

  

    `return $CurrentQuotaInKB`

`}`

  

`Function Get-JDMsolGroupMember {`

        `param(`

            `[CmdletBinding(SupportsShouldProcess=$true)]`

            `[Parameter(Mandatory=$true, ValueFromPipeline=$true,ValueFromPipelineByPropertyName=$true,Position=0)]`

            `[ValidateScript({Get-MsolGroup -ObjectId $_})]`

            `$ObjectId,`

            `[switch]$Recursive`

        `)`

        `begin {`

            `$MSOLAccountSku = Get-MsolAccountSku -ErrorAction Ignore -WarningAction Ignore`

            `if (-not($MSOLAccountSku)) {`

                `throw "Not connected to Azure AD, run Connect-MsolService"`

            `}`

        `}`

        `process {`

            `Write-Verbose -Message "Enumerating group members in group $ObjectId"`

            `[array]$UserMembers = Get-MsolGroupMember -GroupObjectId $ObjectId -MemberObjectTypes User -All`

            `if ($PSBoundParameters['Recursive']) {`

                `$GroupsMembers = Get-MsolGroupMember -GroupObjectId $ObjectId -MemberObjectTypes Group -All`

                `if ($GroupsMembers) {`

                    `Write-Verbose -Message "$ObjectId have $($GroupsMembers.count) group(s) as members, enumerating..."`

                    `$GroupsMembers | ForEach-Object -Process {`

                        `Write-Verbose "Enumerating nested group $($_.Displayname) ($($_.ObjectId))"`

                        `$UserMembers += Get-JDMsolGroupMember -Recursive -ObjectId $_.ObjectId`

                    `}`

                `}`

            `}`

            `Write-Output ($UserMembers | Sort-Object -Property EmailAddress -Unique)`

        `}`

        `end {`

        `}`

    `}`

  
  

`if (Get-Module -Name ExchangeOnlineManagement) {`

    `Write-Host "Exchange online Module installed proceeding to next steps"`

    `#Write-Output  "Exchange online Module installed proceeding to next steps"| Out-File $OutPutFolder -Append`

    `Start-Sleep -Seconds 2`

`}`

`else {`

    `Write-Host "Exchange online Module does not exist needs to be installed to proceed "`

    `#Write-Output  "Exchange online Module does not exist needs to be installed to proceed " | Out-File $OutPutFolder -Append`

    `Start-Sleep -Seconds 2`

    `$isadmin = (New-Object Security.Principal.WindowsPrincipal([Security.Principal.WindowsIdentity]::GetCurrent())).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)`

    `if ($isadmin)`

    `{   Write-Host "Installing Exchange online Module .... "`

    `#Write-Output "Installing Exchange online Module .... " | Out-File $OutPutFolder -Append`

        `Install-module ExchangeOnlineManagement -Force`        

    `}`

    `else {`

        `Write-Host "It Appears you did not run the script with administrative rights please run the script with administrative rights to proceed"`

       `# Write-Output "It Appears you did not run the script with administrative rights please run the script with administrative rights to proceed" | Out-File $OutPutFolder -Append`

        `Pause`

    `}`

`}`

`if (Get-Module -Name MSOnline) {`

        `Write-Host "MSOnline online Module installed proceeding to next steps"`

       `# Write-Output "MSOnline online Module installed proceeding to next steps" | Out-File $OutPutFolder -Append`

        `Start-Sleep -Seconds 2`

    `}`

`else {`

        `Write-Host "MSOnline does not exist needs to be installed to proceed "`

       `# Write-Output "MSOnline does not exist needs to be installed to proceed " | Out-File $OutPutFolder -Append`

        `$isadmin = (New-Object Security.Principal.WindowsPrincipal([Security.Principal.WindowsIdentity]::GetCurrent())).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)`

        `if ($isadmin)`

        `{`

            `Install-module MSOnline -Force`

        `}`

        `else {`

            `Write-Host "It Appears you did not run the script with administrative rights please run the script with administrative rights to proceed"`

        `#    Write-Output "It Appears you did not run the script with administrative rights please run the script with administrative rights to proceed" | Out-File $OutPutFolder -Append`

            `Pause`

        `}`

  

    `}`

    `EditMailboxQuotas`

**The script is provided “AS IS” with no guarantees, no warranties, and it confer no rights.**
