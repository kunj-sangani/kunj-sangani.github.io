---
weight: 4
title: "How To Setup Environment For SharePoint Framework In macOS"
date: 2020-07-03T21:57:40+08:00
lastmod: 2020-07-03T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "Learn how to set up SharePoint Framework (SPFx) for cross-platform SharePoint development with ReactJS, VueJS, and Angular on Mac OS."

tags: ["SPFx", "SharePoint Online", "macOS"]
categories: ["SPFx"]

lightgallery: true
---

Introduction
------------

SharePoint Framework is one of the most loved development frameworks for the SharePoint developer, as it is cross-platform and completely based on front end technologies like reactjs, vuejs, and angular.

Let's understand how we can set up SharePoint Framework (SPFx) in Mac OS.

Step 1 - Install Node.js
------------------------

The first step for setting up our development environment in Mac OS is to install Node.js which is the building block. To install Node.js, follow the below steps.

We will use nvm to install nodejs. nvm is a tool that will help us to switch between node versions seamlessly.

1.  _curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash_  
    ![](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-environment-for-sharepoint-framework-in-macos/Images/InstallNVM.png)
2.  export NVM\_DIR="$HOME/.nvm"
3.  \[ -s "$NVM\_DIR/nvm.sh" \] && \\. "$NVM\_DIR/nvm.sh"
4.  \[ -s "$NVM\_DIR/bash\_completion" \] && \\. "$NVM\_DIR/bash\_completion"  
    ![How To Setup Environment For SharePoint Framework In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-environment-for-sharepoint-framework-in-macos/Images/2_InstallNVM.png)
5.  nvm install 10.21.0  
    ![How To Setup Environment For SharePoint Framework In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-environment-for-sharepoint-framework-in-macos/Images/3_Install Node.png)

_Alternate way without using nvm:_

1.  Navigate to [here](https://nodejs.org/dist/latest-v10.x/).
2.  Download the pkg file for node 10
3.  Install the downloaded pkg file

Once node is installed we can start to install other dependencies for SharePoint Framework. To test whether the node is installed correctly, run the below command:

_node -v_

_npm -v_

Step 2 - Install gulp and Yeoman
--------------------------------

Now let us install gulp and yeoman where gulp is a task runner and yeoman is a scaffolder. Use the below command:

> npm install gulp yo —global

![How To Setup Environment For SharePoint Framework In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-environment-for-sharepoint-framework-in-macos/Images/4_InsallYoGulp.png)

Step 3 - Install Yeoman SharePoint generator
--------------------------------------------

Now let's install the Yeoman SharePoint generator which will help in creating a client-side solution. We can even use the PnP SharePoint Framework generator. To learn more about PnP Yeoman generator, check out this [link](https://pnp.github.io/generator-spfx/).

Use the below command to install the Yeoman SharePoint generator:

> npm install @microsoft/generator-sharepoint --global 

![How To Setup Environment For SharePoint Framework In MacOS](https://f4n3x6c5.stackpathcdn.com/article/how-to-setup-environment-for-sharepoint-framework-in-macos/Images/5_InstallGenerator.png)

Step 4 - Download and Install Visual Studio Code
------------------------------------------------

Now we are ready to create our SharePoint Framework Project. For that, we are required to have a code editor. We can use one of my favourite editors, Visual Studio Code  
  
To install VS Code, navigate to this [link](https://code.visualstudio.com/download), download the pkg file, and install it.

Conclusion
----------

It is very easy to set up the environment for SharePoint Framework, and hence, it has become very popular among developers. Tooling is cross-platform, so we can use any OS of our choice.