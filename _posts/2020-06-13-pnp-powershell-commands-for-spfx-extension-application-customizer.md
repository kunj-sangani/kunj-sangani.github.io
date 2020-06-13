---
date: 2020-06-13 09:45:41
layout: post
title: "PnP PowerShell Commands for SPFx Extension Application Customizer"
subtitle:
description:
image:
optimized_image: /assets/img/Blogs/Blog3/2nd.PNG
category:
tags:
  - SharePoint
  - Powershell 
author:
paginate: false
---

# Introduction

This blog would help in adding, updating, deleting, fetching all the SPFx Extension application customizer using PnP PowerShell commands.

### Prerequisite

To install PnP PowerShell for the Current User, use the below command if you are using Windows 10 and SharePoint Online as SharePoint Environment. Open Windows PowerShell and run all the below commands.

```js
Install-Module SharePointPnPPowerShellOnline -Scope CurrentUser     
```

### Connect to site

To connect to the SharePoint Online Site collection, use the below command:

```js
Connect-PnPOnline –Url https://yoursite.sharepoint.com     
```

## Get

Get all the SPFx Extension application customizers present in the site collection

```js
Get-PnPApplicationCustomizer     
```
![placeholder](/assets/img/Blogs/Blog3/2nd.PNG)

## Add

Add an SPFx Extension application customizer

```js
Add-PnPApplicationCustomizer -Title "CollabFooter" -ClientSideComponentId 87b7c035-6112-4ad2-b33a-f302211976e7 -ClientSideComponentProperties "{`"testMessage`":`"Test message`",`"Bottom`":`"ThisisforTesting`"}"       
```

![placeholder](/assets/img/Blogs/Blog3/5th.PNG)

#### <ins>Note</ins>
We can get ClientSideComponentId from the .manifest.json file as displayed in the screenshot below

![placeholder](/assets/img/Blogs/Blog3/4th_1.PNG)

## Update

Updated the Properties of the SPFx application customizer. This is very helpful if we want to change the properties value
 
For example, if we create a footer for all the site collection in our tenant and we have a requirement of having different text in footer then we can run this command to update the value

```js
Set-PnPApplicationCustomizer -ClientSideComponentId 87b7c035-6112-4ad2-b33a-f302211976e7 -Scope web -ClientSideComponentProperties "{`"testMessage`":`"Test message`",`"Bottom`":`"Thistextisupdated`"}"        
```

![placeholder](/assets/img/Blogs/Blog3/6th.PNG)

![placeholder](/assets/img/Blogs/Blog3/7th.PNG)

## Remove

To remove the SPFx application customizers, use the below command:

```js
Remove-PnPApplicationCustomizer -ClientSideComponentId 87b7c035-6112-4ad2-b33a-f302211976e7 -Scope web         
```

![placeholder](/assets/img/Blogs/Blog3/8th.PNG)