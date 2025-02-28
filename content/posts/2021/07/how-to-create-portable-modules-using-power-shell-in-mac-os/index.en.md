---
weight: 4
title: "How To Create Portable Modules Using PowerShell In macOS"
date: 2021-07-14T21:57:40+08:00
lastmod: 2021-07-14T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "The article explains how to create custom PowerShell modules using PowerShell 7 and .NET Core, which allows developers to use PowerShell on all platforms, and provides guidance on installing PowerShell on macOS."

canonicalurl: "https://www.c-sharpcorner.com/article/how-to-create-portable-modules-using-powershell-in-macos2/"

tags: ["PowerShell", "macOS"]
categories: ["PowerShell"]

lightgallery: true
---

Introduction
------------

With the introduction of PowerShell 7 it is very easy to use PowerShell across all the platforms and so we will want to create our own custom PowerShell modules as well.

To do so it is very easy with the introduction of .net core --  truly Microsoft is being platform agnostic in terms of development.

So let us understand how we can create our own custom module for PowerShell.

Before starting let us install PowerShell in macOS.

Install PowerShell in macOS
---------------------------

**Step 1 Install Homebrew**
---------------------------

To run PowerShell we needto have Homebrew. It is a package manager which is used for installing packages which are missing in macOS.

Use the below command in the terminal to install Homebrew.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
![How To Create Portable Modules Using PowerShell In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-create-portable-modules-using-powershell-in-macos2/Images/How To Create Portable Modules Using PowerShell In MacOS.jpg) 

**Step 2 Install PowerShell**
---------------------------

Once we have installed Homebrew now we are good to install PowerShell. This is also recommended by Microsoft ;for more details check the [link](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-7).

Use the below command to install PowerShell.
```
brew cask install powershell
```

To test if we have installed it properly or not use the below command:
```
pwsh
```


Create PowerShell Custom Module
-------------------------------

To Create Custom module we will use .net core for creating it. Assuming that we have installed .net core already in the environment now let us install it. 

  
To install .net core module for creating custom PowerShell module use the below command:
```
dotnet new --install Microsoft.PowerShell.Standard.Module.Template::0.1.3
```

Now let use create a project for creating custom PowerShell module. To scaffold the project use the below command:
```
dotnet new psmodule
```

There would be bin, obj folder and .csproj and .cs file in the project. Open the code in Visual Studio to check the code.

As displayed in the image below to create a custom command we have to inherit the class PSCmdlet and the below three methods define the execution.

*   BeginProcessing() - This method is called when the pipeline starts execution.
*   ProcessRecord() - This method is called for every input which is passed to the command.
*   EndProcessing() - This method is called at the end of pipeline execution.

To build the project use the below command:
```
dotnet build
```
Open PowerShell using the below command:
```
pwsh
```
Once we have build the module now we can import it to test our module

Use the below command to import the module:
```
Import-Module .\\bin\\debug\\netstandard2.0\\powershellmodule.dll
```

```
Test-SampleCmdlet -FavoriteNumber 3 -FavoritePet Dog
```
![How To Create Portable Modules Using PowerShell In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-create-portable-modules-using-powershell-in-macos2/Images/How%20To%20Create%20Portable%20Modules%20Using%20PowerShell%20In%20MacOS.jpg)

Conclusion
----------

It is very easy to create custom PowerShell module using any platform of our choice. This module is reusable so once we have developed our custom module we can use it anywhere we want.