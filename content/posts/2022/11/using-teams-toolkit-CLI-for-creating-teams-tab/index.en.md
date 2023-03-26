---
weight: 4
title: "Using Teams Toolkit CLI For Creating Teams Tab"
date: 2022-11-07T21:57:40+08:00
lastmod: 2022-11-07T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "This article explores how to create Tabs in Microsoft Teams using the Teams Toolkit."

tags: ["Teams Tab", "TeamsFx", "Teams Toolkit"]
categories: ["Microsoft Teams"]

lightgallery: true
---
Introduction
------------

There are multiple ways to create a Teams Tab project such as using the Teams Toolkit extension for Visual Studio code, Yeoman generator for Teams, etc. Among these options, one such option is to use Teams Toolkit CLI before creating a Teams Tab project using Team Toolkit CLI let's understand what Teams Toolkit CLI is.

What is Teams Toolkit CLI?
--------------------------

Teams Toolkit CLI is a command line interface that helps in Teams application development. It is available as a part of the node package manager because it is straightforward to use Teams Toolkit CLI in CI/CD pipelines to build and deploy our Teams application. The best part of Teams Toolkit CLI is it is an open-source project. So you can check out the source code using the below link.

[TeamsFx/packages/cli at dev · OfficeDev/TeamsFx (github.com)](https://github.com/OfficeDev/TeamsFx/tree/dev/packages/cli)

How to Install Teams Toolkit CLI?
---------------------------------

To install Teams Toolkit CLI pre-requisite is to have NodeJS installed and use the LTS version of NodeJS if not already installed. Once your environment has NodeJS use the below command to install Teams Toolkit.

```
npm install -g @microsoft/teamsfx-cli
```

![Install Teams Toolkit CLI](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%202.png)

To List, all the commands available as part of Teams Toolkit CLI use the below command

```
teamsfx -h
```

![Install Teams Toolkit CLI](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%203.png)

How to scaffold a new Teams Tab project?
----------------------------------------

To create a new project using Teams Toolkit CLI there are two different ways 

1.  Using an Interactive way
2.  Using a non Interactive way

We will look into both ways

### 1) Using an Interactive way

To leverage the interactive way of creating a Teams Tab project run the below command and then follow the below-specified steps
```
teamsfx new
```
1. Select Create a new Teams app

![How to scaffold a new Teams Tab project](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%204.png)

2. Select Capabilities as Tab by using the arrow keys

3. Select the programming language as Typescript

4. Select the Workspace folder  ./

Note: you can provide your path to the folder where you want the scaffolded project to be present

5. Provide the application name as TeamsTabUsingCLI

![How to scaffold a new Teams Tab project](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%208_1.png)

Once we complete the above steps a new Teams Tab project will be created with the required files as displayed in the image below 

![How to scaffold a new Teams Tab project](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%209.png)

### 2) Using non Interactive way

To use a non-interactive form run the below command
```
teamsfx new --interactive false --capabilities "tab" --programming-language "typescript" --folder "./" --app-name TeamsTabUsingCLI
```

![How to scaffold a new Teams Tab project](https://f4n3x6c5.stackpathcdn.com/article/using-teams-toolkit-cli-for-creating-teams-tab/Images/Image%2010.png)

So by using just one command we can create a Teams Tab project with the required files

Conclusion
----------

Teams Toolkit CLI also known as TeamsFx CLI is a beneficial CLI tool for creating, building, and deploying Teams applications. It even helps create projects from samples provided by Microsoft