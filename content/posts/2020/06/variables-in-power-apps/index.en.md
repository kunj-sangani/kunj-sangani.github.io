---
weight: 4
title: "Variables In PowerApps"
date: 2020-06-23T21:57:40+08:00
lastmod: 2020-06-23T21:57:40+08:00
draft: false
author: "Kunj Sangani"
authorLink: "https://www.linkedin.com/in/kunj-sangani/"
description: "The article explains how the introduction of variables in PowerApps has made it a powerful no-code tool for easy development, and provides a comprehensive guide on how to use variables in PowerApps."

tags: ["PowerApps"]
categories: ["PowerApps"]

lightgallery: true
---

Introduction
------------

With the introduction of PowerApps, more and more users are moving into PowerApps development for easy and no-code development tools. However, users from a coding background found it difficult, as there were no variables in PowerApps. Not now, with the introduction of variables in PowerApps, it has become a Powerful no-code tool. So let's understand PowerApps Variables and how they works.

Types of Variables
------------------

Just as we have different scopes of variables in the programming language in PowerApps, we also have different scoped variables the three different types of variables which are available in PowerApps, as mentioned below:

| Variables types | Scope | Description | Functions related to variable |
| --------------- | ----- | ----------- | ----------------------------- |
| Global variable | App | This is similar to a global variable in programming language and it can hold a number, text string, Boolean, record, table, etc as a data value | Set |
| Context variables | Screen | This is similar to the parameter which we pass to methods or procedure and this variable can be referenced from only one screen | UpdateContext |
| Navigate | Collections | This variable can store table which can be referenced across anywhere in the app | Collect, ClearCollect |

Creating and Removing Variables
-------------------------------

In all programming languages, we declare a variable before using the variable but such is not the case with PowerApps. So, in PowerApps we never declare variables explicitly. In terms of typing, PowerApps has implicit typing. So, we do not require to specify the type of variable for example int a = 0 in programming language indicated the variable is of type integer but in PowerApps we do not require to specify the type of variable.

To create a variable, we just need to run the function:
```
Set(global Variable, "Hello World")  
Update Context({context Variable:"Hello World"})  
Collect( collection Variable, "Hello World" )     
```
Where X is the variable name and Hello World is the value of that variable.

For removing the variable, we are required to remove all Set, UpdateContext, Navigate, Collect, or ClearCollect functions that implicitly establish the variable. If these functions are not present then there is no variable. We must even remove the variable reference if used in other places.

To properly understand all the three different types of the variable, we can create a small example.

Global Variable
---------------

To understand the global variable, we can create a sample canvas app. Let's create it using the below step:

1.  Navigate to [here](https://make.powerapps.com).
2.  Click on create
3.  Select Canvas app from blank as a type of PowerApps
4.  Provide the name of the app as GlobalVariable and select the format as Tablet
5.  Add a text input, label, and button from the insert table
6.  Select the button and update the formula in OnSelect  
```    
Set(global Variable,TextInput1.Text)  
```   

7.  Now select the label field and update the formula in the Text field  
```    
global Variable  
```    

8.  To check all global variables in PowerApps click on File, then click on Variables. This would display all the global variables present in the app. To check all the references, click on the variable. This would display where the variable definition is present and where it is used

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/1_GlobalVariable.png)

This app will update the variable value as entered in the text field and display it in the label accordingly.

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/globalVariable.gif)

 

Context Variable
----------------

To understand the context variable, we can create a sample canvas app. Let's create it using the below step:

1.  Navigate to [here](https://make.powerapps.com).
2.  Click on create
3.  Select Canvas app from blank as a type of PowerApps
4.  Provide the name of the app as contextVariable and select the format as Tablet
5.  Add a text input, label, and button from the insert table
6.  Select the button and update the formula in OnSelect  
```    
Update Context({contextVariable:TextInput1.Text})  
```    

7.  Now select the label field and update the formula in the Text field  
```    
context Variable  
```    

8.  To check all context variables in PowerApps click on File, then click on Variables. This displays all the screens present in the app. To check all context variables inside the screen, select the screen. To check all the references, click on the variable. This would display where the variable definition is present and where it is used.

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/2_ContextVariable.png)

This app will update the variable value as entered in the text field and display it in the label accordingly.

 

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/contextVariable.gif)

Collections Variable
--------------------

To understand the collections variable, we can create a sample canvas app. Let's create it using the below step:

1.  Navigate to [here](https://make.powerapps.com).
2.  Click on create
3.  Select Canvas app from blank as a type of PowerApps
4.  Provide the name of the app as collection variable and select the format as Tablet
5.  Add a five text input, two-button and Data table from the insert table
6.  Select the button and update the formula in OnSelect and change the text to Submit Collection  
```    
Collect(formDataTemp,{Name:TextInput1.Text,temp1:TextInput2.Text,temp2:TextInput3.Text,temp3:TextInput4.Text,temp4:TextInput5.Text})  
```    

7.  Now select the Data table and update the formula in Items field  
```    
formDataTemp  
```    

8.  To clear the data from the collection, select the second button and change the text to Clear Data and update the OnSelect field with the below formula  
```
Clear(formDataTemp)  
``` 

9.  To check all collections variables in PowerApps click on File, then click on Collections. This would display all the collections variables present in the app. It would also display the first five items in the collections variable

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/3_Collections.png)

This app will update the variable value as entered in all the text fields and display it in the data table accordingly. To clear the data, we can click on the Clear Data button.

![Variables In PowerApps](https://f4n3x6c5.stackpathcdn.com/article/variables-in-powerapps/Images/collectionVatiable.gif)

Conclusion
----------

With the introduction to variables in PowerApps, it has become a powerful tool for no-code application. Use variables in your application wisely, as they use the application memory.