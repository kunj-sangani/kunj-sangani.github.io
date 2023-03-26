---
weight: 4
title: "How To Run PnP PowerShell In MacOS"
date: 2021-08-16T21:57:40+08:00
lastmod: 2021-08-16T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Using PowerShell in macOS and automating Microsoft 365 tasks with cross-platform PnP PowerShell."

tags: ["macOs", "PnP PowerShell"]
categories: ["PowerShell"]

lightgallery: true
---

Introduction
------------

There may be requirements where we need to use PowerShell and if we are using macOS then the question becomes how we can use PowerShell in macOS. But with Microsoft's new approach of creating everything open source and cross-platform, we can achieve this very easily. But along with this, we would even have a look at how we can use cross-platform PnP PowerShell to automate our Microsoft 365 related tasks.

To run PowerShell Script in Mac OS follow the below steps,

Step 1 - Install Homebrew
-------------------------

To run PowerShell we need to have Homebrew; it is a package manager which is used for installing packages that are missing in macOS.

Use the below command in the terminal to install Homebrew,
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
![How to run PnP PowerShell in macOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-run-pnp-powershell-in-macos/Images/How%20to%20run%20PnP%20PowerShell%20in%20macOS.jpg)

Step 2 - Install PowerShell
---------------------------

Once we have to install Homebrew now we are good to install PowerShell. This is also recommended by Microsoft for more details check the [link](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-7).

Use the below command to install PowerShell,
```
    brew cask install powershell
```

![How to run PnP PowerShell in macOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-run-pnp-powershell-in-macos/Images/How%20to%20run%20PnP%20PowerShell%20in%20macOS1.png)

To test if we have installed properly or not use the below command,
```
    pwsh
```
![How to run PnP PowerShell in macOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-run-pnp-powershell-in-macos/Images/How%20to%20run%20PnP%20PowerShell%20in%20macOS2.jpg)

Now let us walk through step by step process of using PnP.PowerShell is a cross-platform module for working with Microsoft 365 related activities.

Step 1 - Register the package source
------------------------------------

This step is required because we would like to add PowerShell salary as the package source provider so that we can install the module directly without an issue to do so run the below command,
```
Register-PackageSource -Name PSNuGet -Location https://www.powershellgallery.com/api/v2 -ProviderName NuGet

```
![How to run PnP PowerShell in macOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-run-pnp-powershell-in-macos/Images/How%20to%20run%20PnP%20PowerShell%20in%20macOS3.png)

Now we have set the package source so that we can search for our required PnP.PowerShell module which we can use for our Microsoft 365 related PowerShell activities.

Step 2 - Install PnP PowerShell
-------------------------------

Now to work with PnP PowerShell we need to install dependencies for that we would require to run the below command,
```
Install-Module PnP.PowerShell
```
![How to run PnP PowerShell in macOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-run-pnp-powershell-in-macos/Images/How%20to%20run%20PnP%20PowerShell%20in%20macOS4.png)

Now we are ready to use PnP PowerShell let us test that by connecting to any site collection.

Step 3 - Connect to SharePoint and get the web
----------------------------------------------

To connect to SharePoint we can use the Connect-PnPOnline command and we can even use web login or device login if in case you are using Multi-factor authentication.
```
Connect-PnPOnline -Url https://testinglala-admin.sharepoint.com
```

To get the web once we have logged in we can just use Get-PnPWeb which would provide us the web to the URL we have logged in using connect cmdlet.
```
Get-PnPWeb
```

Conclusion
----------

With the help of PowerShell being cross-platform, we can use any operating system of our choice to leverage PowerShell and the best part is, it is even available in docker image as well.

The previous version of PnP PowerShell, which was SharePointPnPPowerShellOnline, was not cross-platform, but thanks to CSOM for .net core has helped us with the new Module PnP.PowerShell which is now cross-platform so we can leverage it to automate our tasks irrespective of the operating system we are working with.