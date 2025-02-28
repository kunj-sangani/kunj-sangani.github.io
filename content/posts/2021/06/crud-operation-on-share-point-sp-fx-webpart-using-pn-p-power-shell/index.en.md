---
weight: 4
title: "CRUD Operation On SPFx Webpart Using PnP PowerShell"
date: 2021-06-08T21:57:40+08:00
lastmod: 2021-06-08T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "The article guides readers on using PnP PowerShell to perform CRUD operations for SharePoint Framework webparts, simplifying maintenance and automation."

canonicalurl: "https://www.c-sharpcorner.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/"

tags: ["SPFx", "PowerShell"]
categories: ["PowerShell"]

lightgallery: true
---

Introduction
------------

With the introduction to SharePoint Framework (SPFx), creating webparts using frontend technologies has been very easy. But what if we want to maintain it and create some automation for adding, updating, or deleting this SPFx webparts? In this scenario, PnP PowerShell helps us with all the CRUD operations for SPFx webparts. So let us check how we can utilize this.

Installation of PnP PowerShell
------------------------------

Installing PnP PowerShell is very easy. There are two methods.

If you are using Windows 10 then running the below command in PowerShell  will install the PnP PowerShell module
```
Install-Module SharePointPnPPowerShellOnline -Scope CurrentUser  
```
Install the executable by downloading it from the below [link](https://github.com/pnp/PnP-PowerShell/releases).

Add SPFx Webpart in Site Page
-----------------------------

Before Running the below commands we need to first connect sitecollection. To do so we can use the below command:
```
Connect-PnPOnline <sitecollection URL> 
```
For adding SPFx webpart there are two options,

1.  We add out of the box webpart
2.  We add custom created webpart

For Adding Out of the box webpart we can run the below command
```
Add-PnPClientSideWebPart -Page TestPage -DefaultWebPartType Image -WebPartProperties '{"imageSource":"https://testinglala.sharepoint.com/sites/Test/SiteAssets/SitePages/mypage/1789636885Scottish-wildcat-in-the-grass.jpg"}'  
```
**Note**

In the –DefaultWebPartType we can use any of the out of the box webpart names.

![CRUD Operation On SharePoint SPFx Webpart Using PnP PowerShell](https://f4n3x6c5.stackpathcdn.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/Images/1_AddWebPart.png)

For Adding Custom SPFx Webpart we can use the below command,
```
Add-PnPClientSideWebPart -Page TestPage -Component "ie11Test"  
```
![CRUD Operation On SharePoint SPFx Webpart Using PnP PowerShell](https://f4n3x6c5.stackpathcdn.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/Images/2_AddWebPart.png)

**Available Parameters**

1.  Page - This parameter accepts a string and we can provide the name of the page where we want to add the webpart
2.  Section - This parameter accepts integer values which indicate in which section we want to add the SPFx webpart
3.  Column - This parameter also accepts integer values and indicate in which column we want to add SPFx webpart
4.  Component - To add custom SPFx webpart we need to use this parameter. We can either provide GUID or the name of the component
5.  DefaultWebPartType - To add Out of the box webpart we can specify the name of the webpart in this parameter
6.  Order - This parameter accepts integer values and indicates the order of the webpart
7.  WebPartProperties - To specify the property of the webpart we can use this field it can be used both in Custom SPFx webpart as well Out of the box webparts

Update SPFx Webpart in Site Page
--------------------------------

To update the SPFx webpart we want to update the webpart properties. This can be easily achieved using the below command:
```
Set-PnPClientSideWebPart -Page TestPage -Identity f6e82e11-f2d7-4bf3-a90a-c7ada44bfbba -PropertiesJson '{"description":"Testing123"}'  
```
![CRUD Operation On SharePoint SPFx Webpart Using PnP PowerShell](https://f4n3x6c5.stackpathcdn.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/Images/3_UpdateWebPart.png)

**Available Parameters**

1.  Identity - This parameter accepts the ID of the webpart. This can accept title or instance id of the webpart.
2.  Page - This parameter accepts a string and we can provide the name of the page where we want to update the webpart.
3.  PropertiesJson - To update the property values we can provide the JSON. In this parameter it's displayed in the command above.
4.  Title - To update the internal title of the webpart

**Note**

This parameter will not update the title which is displayed for the webpart.

Fetch All SPFx Webpart in Site Page
-----------------------------------

To fetch all the webparts present in the Site Page we can use the below command. This would enumerate all the client-side webparts.
```
Get-PnPClientSideComponent -Page TestPage
```
![CRUD Operation On SharePoint SPFx Webpart Using PnP PowerShell](https://f4n3x6c5.stackpathcdn.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/Images/4_GetWebParts.png)

**Available Parameters**

1.  InstanceId - This parameter accepts the ID of the webpart. This can accept title or instance id of the webpart.
2.  Page - This parameter accepts a string and we can provide the name of the page where we want to get the webpart.

Delete SPFx Webpart in Site Page
--------------------------------

To remove any specific instance of the webpart in the site page we can use the below command which would delete the instance from the page.
```
Remove-PnPClientSideComponent -Page Home -InstanceId a2875399-d6ff-43a0-96da-be6ae5875f82  
```
![CRUD Operation On SharePoint SPFx Webpart Using PnP PowerShell](https://f4n3x6c5.stackpathcdn.com/article/crud-operation-on-sharepoint-spfx-webpart-using-pnp-powershell/Images/5_DeleteWebParts.png)

**Available Parameters**

1.  Force - If we want to skip the confirmation for deleting the webpart we can use this parameter
2.  InstanceId - This parameter accepts the ID of the webpart. This can accept title or instance id of the webpart
3.  Page - This parameter accepts a string and we can provide the name of the page where we want to delete the webpart.

Conclusion
----------

From the above commands, we can conclude that for automating the process adding, creating,  updating or deleting the webpart becomes easy using the PnP PowerShell command and it becomes very easy to maintain the Site Pages using these commands. We can even leverage this to automate the modern page creation by adding webparts as per our need using above command.