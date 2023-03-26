---
weight: 4
title: "Teams Toolkit For Creating Microsoft Teams Tab"
date: 2022-11-03T21:57:40+08:00
lastmod: 2022-11-03T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Learn how to create Tabs in Microsoft Teams using the Teams Toolkit with this step-by-step guide."

tags: ["Teams Tab", "TeamsFx", "Teams Toolkit"]
categories: ["Microsoft Teams"]

lightgallery: true
---

Introduction
------------

Microsoft Teams is one of the most used collaborative platforms with 270 million monthly active users as of 2022. Microsoft teams have extensibility capabilities that help developers create their own applications. For this application, we have many application capabilities such as Personal apps, Tabs, Bots, Messaging extensions, Meeting extensions, and Webhook and connectors.

In this article, we will leverage the capability of Tabs in Microsoft teams. We will go through step by step process of creating Tabs using Teams Toolkit.

What is Teams Toolkit?
----------------------

Teams toolkit is an extension in Visual studio code and Visual Studio which helps app development for Microsoft teams simple as it provides templates and samples for all different application capabilities.

To install Teams Toolkit in Visual studio code follow the below steps,

1\. Open Visual studio code

2\. Navigate to the extension section in Visual studio code as displayed in the image below

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/1-TeamsToolkit-1.png)

3\. Search for Teams Toolkit and Install the extension

Once the Teams toolkit is installed Teams toolkit icon would be displayed on the primary bar which is present as left navigation in Visual studio code.

Create Teams Tab using Teams Toolkit
------------------------------------

To create a teams tab using the Teams toolkit use the below steps which would help us create a template for teams tabs that we can extend as per our requirement

1\. Open Visual studio code

2\. Click on the Teams Toolkit icon present on the primary bar of Visual studio code.

**Note**  
This icon will be displayed after the teams toolkit is installed as explained in the above section.

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%202-1.png)

3\. Click on Create a new team app button which would provide two options Create a new Teams app and Start from a sample. We would select Create a new Teams app for this article but if you want to start from a sample you can select that option and explore all the samples provided by Microsoft which caters to specific business use case

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%203-1.png)

4\.  We would have options to select one from different capabilities provided for Teams application development but for this article, we would select the Tabs option as displayed in the image below

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%204-1.png)

5\. We would be provided with two different options for selecting a programming language JavaScript and TypeScript.

For this article let us select TypeScript,

    

6\. Select the workspace folder where we want all our scaffolded files to be present

    

7\. Provide the application name. For this article let us use HelloWorldTeamsTab

Once all the above steps are completed a scaffolded project with different files would be opened in a new Visual studio code instance as displayed in the image below,

![](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image 9-1.png)

Million dollar question is how to test in Microsoft Teams
---------------------------------------------------------

The Teams toolkit helps us here as well. To test out the tab in Microsoft Teams follow the below steps

1\. Navigate to the Teams toolkit extension present on the primary bar of Visual studio code

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%2010-1.png)

2\. Click on the Sign in to Microsoft 365 options present under the Accounts section

3\. If you do not have a developer tenant then you can create one by selecting the option Create a Microsoft 365 testing tenant or clicking on Sign in

4\. Sign-in would happen in the browser so you can provide your email address and password and close the tab which got opened in the browser for login

5\. Please confirm in the Accounts section whether your login was successful or not

6\. Now to test just press the F5 key

7\. A popup for installing an SSL certificate would appear which you should install and let the magic of Teams Toolkit work

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%2014-1.png)

8\. A new browser with Teams would open with our app created using the Teams toolkit as displayed in the image below

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%2016-1.png)

9\. We can select Add to a team and then select the team where we want to test

10\. A tab configuration page would open we can select save and see our app in the Teams context

Project Structure 
------------------

All our code is present under the tabs folder which has an src folder that contains all our react components that are created as a part of the template we can change this code and update it as per our requirement

Conclusion
----------

Using Teams Toolkit it becomes very easy to create Teams application as it provides us with templates and the best part is we can even use the templates which are created under the templates folder for deploying it to Azure as these are bicep templates. Even implementing SSO in the Teams tab is very simple using the Teams Toolkit

![Teams Toolkit for Creating Microsoft Teams Tab](https://f4n3x6c5.stackpathcdn.com/article/teams-toolkit-for-creating-microsoft-teams-tab/Images/Image%2019-1.png)