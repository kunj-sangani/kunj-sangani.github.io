---
weight: 4
title: "How To Setup Environment For Yeoman Teams Generator"
date: 2020-06-03T21:57:40+08:00
lastmod: 2020-06-03T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Setting up the PnP Yeoman scaffolder for Teams applications to extend the capabilities of Microsoft Teams and creating a sample bot for better collaboration."

canonicalurl: "https://www.c-sharpcorner.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/"

tags: ["Yo Teams", "Teams Tab"]
categories: ["Microsoft Teams"]

lightgallery: true
---

**Introduction**
----------------

With the increase in Microsoft Teams adoption, companies are switching to Microsoft team's development for extending the capabilities for Microsoft Teams for better collaboration. To make this development easy to use and to centralize it, the PnP team has generator a yeoman scaffolder for Teams applications. So let's look into setting up the environment and creating a sample bot for Microsoft Teams.

Environment Setup 
------------------

**Step 1 - Install nvm-windows**
----------------------------

We will use nvm-windows for installing Node.js because we can easily use a different version of Node.js and upgrading Node.js or using the previous version using nvm-windows is very easy. However, we can skip this step if we want to install Node.js directly.

Download nvm-windows from below link use nvm-setup.zip file for more information on nvm-windows [check my blog](https://www.c-sharpcorner.com/blogs/how-to-use-multiple-nodejs-version-in-windows-os)

_https://github.com/coreybutler/nvm-windows/releases_ 

**Step 2 - Install the latest version of Node.js**
----------------------------

We can install Node.js directly from the below link:

  
_https://nodejs.org/en/download/_

If we are using nvm-windows, use the below command to install the latest version of Node.js:

_nvm install latest_

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/temp/81001/Images/2_Install%20Latest.png)

**Step 3 - Install Yeoman and Gulp CLI**
----------------------------

To scaffold projects using the Teams generator, we need to install the Yeoman tool as well as the Gulp CLI task manager.

Using the below command, we are able to install the above dependencies.

_npm install yo gulp-cli --global_

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/temp/81001/Images/2_yoandgulp.png)

**Step 4 - Install Yeoman generator for Microsoft Teams apps**
----------------------------

Now we need to install the Yeoman generator for Microsoft Teams. Use the below command to install it.

_npm install generator-teams --global_

To install the preview version which would provide the feature which is still in preview, use the below command:

_npm install generator-teams@preview --global_

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/temp/81001/Images/3_generator.png)

Generate Bot Project for Microsoft Teams
----------------------------------------

**Step 1 - Create a Folder** 
----------------------------

Let's create a bot for Microsoft teams. To create the bot, we would use yeoman teams generator for scaffolding the project:

To create a project for use of the project files, use the below command to create a folder:

_mkdir yoTeamsBot_

  
Now let's navigate to the created folder using the below command:

_cd yoTeamsBot_

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/temp/81001/Images/1_CreateFolder.png)

**Step 2 - Create an Azure AD Application and Register a New Bot**
----------------------------

To create a Bot, we are required to register an Azure AD application. To register a new application, use the below steps:

*   Navigate to [here](https://portal.azure.com/).
*   Select Azure Active Directory and from the left navigation, select App registrations.
*   Click on New Registration, as displayed in the image below.

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/5_AzureApp.png)

*   Add an appropriate name for the application and register the application (Copy the Client ID that is generated).

**

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/6_Form.png)

**

*   Once the application is created, click on Certificates and Secret and Create a Client Secret Copy. This we use while generating the application.

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/7_1_CreateSecret.png)

Register for Bot Application
----------------------------

We are now required to register the Bot Application for registration. Follow the below steps:

*   Navigate to [here](https://dev.botframework.com/bots/new).
*   Fill up the form with appropriate data, as displayed in the image below.
*   Use the Azure Ad client ID which we have created in the above process. In the field, paste your app ID below to continue.

 **![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/12_RegisterBot.png)

**Step 3 - Create a Bot application**  
----------------------------

Create the project using the below command:

_yo teams_

There are multiple questions that could be asked before generating the project. Use the below answers for generating the project.

*   What is your solution name? yo-teams-bot
*   Where do you want to place the files? Use the current folder
*   Title of your Microsoft Teams App project? yoTeamsBot  
    
*   Your (company) name? (max 32 characters) <Use your Company Name>  
    
*   Which manifest version would you like to use? v1.6  
    
*   Enter your Microsoft Partner ID, if you have one? (Leave blank to skip)
*   What features do you want to add to your project? A bot
*   The URL where you will host this solution? https://yoteamsbot.azurewebsites.net  
    
*   Would you like to show a loading indicator when your app/tab loads? No  
    
*   Would you like to include the Test framework and initial tests? No  
    
*   Would you like to use Azure Applications Insights for telemetry? No  
    
*   What type of bot would you like to use? A new bot Framework bot  
    
*   What is the name of your bot? yoTeamsBot  
    
*   What is the Microsoft App ID for the bot? It's found in the Bot Framework portal (_https://dev.botframework.com_). <Azure AD Application ID> Use the above step to create Azure AD Application ID
*   Do you want to add a static tab to your bot? No  
    
*   Do you want to support file upload to the bot? No  
    

 **![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/4_yoTeams.png)

**Step 4 - Update the Code** 
----------------------------

Open the .env file. Add the MICROSOFT\_APP\_PASSWORD value with the client secret that we created while generating the Azure AD application.

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/8_Updatecode.png)

**Step 5 - Run the Bot Application**
----------------------------

_gulp ngrok-serve_

**Step 6 - Add the Manifest to MS Teams**
----------------------------

Navigate to the Package folder inside the package folder which is created once we run the above command. A zip file is created inside the package folder which is used in MS Teams for registering our Bot. This is the manifest file that is generated.

Open MS Teams Navigate to Apps section in Navigation. On the left search for App Studio, add open App Studio, and select Manifest editor.

Click on Import an existing app and select the Manifest file present in the package folder.

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/9_AddManifesto.png)

Once the import is completed, install the application by selecting the application and Click on Test and Distribution present in the Finish Section 

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/10_InstallApp.png)

**Outcome**

Now we can use our code to send and receive a message in MS Teams!

![How To Setup Enviornment For Yeoman Teams Generator](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-enviornment-for-yeoman-teams-generator/Images/11_Outcome.png)

Conclusion
----------

Using the PnP Teams generator for creating Microsoft Teams application is very easy and we can leverage open source technologies and Javascript development.

We can even develop Tabs, an outgoing webhook, a connector, a messaging extension for Microsoft Teams.